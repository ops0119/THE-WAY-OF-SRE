## 01-阿里云GPU服务器

## 1、使用GPU流程​

1. **硬件虚拟化层（分卡）：** 决定如何将物理GPU分配给虚拟机（VM）。区分为“**直通**”或“**半虚拟化**”。
2. **驱动安装层（认卡）：** 在虚拟机或物理机内部安装NVIDIA驱动，让操作系统能识别并控制GPU。
3. **应用开发层（用卡）：** 安装CUDA、cuDNN等软件库，让深度学习框架（如PyTorch、TensorFlow）能调用GPU进行计算。

## 2、硬件虚拟化为什么要有“直通”和“半虚拟化”？

这两种技术主要解决**物理GPU资源如何分配给虚拟机**的问题，它们各有优劣，适用于不同场景：

### 2.1 GPU直通 (GPU Passthrough)

- **原理：** 利用IOMMU/VT-d技术，将物理GPU直接“挂载”到某一个虚拟机上。虚拟机独占这块显卡，几乎拥有所有的硬件权限。
- **特点：**
  - **性能极高：** 性能损耗通常小于5%，接近物理机（裸金属）。
  - **独占性：** 一块物理GPU一次只能给一个虚拟机用。
  - **兼容性好：** 支持标准的NVIDIA Tesla驱动。
- **缺点：** 资源利用率低（如果虚拟机没跑满，剩余算力也无法被别人使用），且通常不支持虚拟机热迁移。
- **适用场景：** AI模型训练、高性能计算（HPC）、对性能要求极高的任务。

### 2.2 半虚拟化 / vGPU (如NVIDIA GRID, MIG)

- **原理：** 通过软件或硬件切片技术（如SR-IOV、MIG），将一块物理GPU切割成多个“虚拟GPU”（vGPU），分配给不同的虚拟机同时使用。
- **特点：**
  - **资源利用率高：** 多个用户共享一张卡，成本更低。
  - **灵活性强：** 可以动态调整每个vGPU的显存和算力配额。
  - **支持热迁移：** 虚拟机可以在物理机之间迁移。
- **缺点：** 有性能损耗（通常在10%-15%左右），且往往需要购买昂贵的商业授权（License）。
- **适用场景：** 云桌面、图形工作站（VDI）、推理服务（Inference）、多租户环境。



### 2.3 核心差异对比表

| 维度         | GPU直通 (Passthrough)     | 半虚拟化 / vGPU                     |
| :----------- | :------------------------ | :---------------------------------- |
| **资源分配** | 1张卡 -> 1个虚拟机 (独占) | 1张卡 -> N个虚拟机 (共享/切片)      |
| **性能损耗** | 极低 (<5%)                | 中等 (10-15%)                       |
| **驱动类型** | Tesla Driver (免费)       | GRID Driver (通常需付费授权)        |
| **主要优势** | 极致性能，低延迟          | 高密度，低成本，灵活调度            |
| **典型用途** | **训练** (Training)       | **推理/图形** (Inference/Rendering) |

## 3、阿里云GPU分类+安装



**GPU虚拟化型**(半虚拟化):一般是sgn、vgn开头的规格族

实例的CPU和网络资源采用共享模式提供，最大化利用底层资源。内存和GPU显存采用独享模式提供

GPU分片表示1块GPU分成多片，每个实例上使用1片。例如：

`NVIDIA A10 * 1/12`中的`NVIDIA A10`表示GPU卡型号；`1/12`表示GPU分片，即1块GPU分成12片，每个实例上使用1片。

**GPU计算型（**直通）：gn开头

**裸金属服务器**:ebmgn开头

| 类别         | **GPU计算型**    | **GPU虚拟化型**    | **裸金属服务器**    |
| ------------ | ---------------- | ------------------ | ------------------- |
| 代表规格族   | **gn6i, gn7i**   | **vgn7i, sgn8ia**  | **ebmgn6, ebmgn8v** |
| 模式         | **直通**         | **半虚 (vGPU)**    | **物理直通**        |
| 驱动类型     | **Tesla Driver** | **GRID Driver**    | **Tesla Driver**    |
| 驱动安装难度 | 低 (标准流程)    | 中 (需注意License) | 低 (标准流程)       |
| CUDA安装     | 需要             | 需要               | 需要                |





<table border="1" cellpadding="8" cellspacing="0" style="border-collapse: collapse; width: 100%;">   <thead>     <tr>       <th>虚拟化分类</th>       <th>系统类别</th>       <th>驱动安装</th>       <th>驱动类型</th>       <th>规格代表</th>     </tr>   </thead>   <tbody>     <tr>       <td rowspan="2">GPU计算</td>       <td>windows</td>       <td>仅创建后</td>       <td>Tesla+GRID（T4、A10）</td>       <td rowspan="2">gn开头规格</td>     </tr>     <tr>       <td>linux</td>       <td>创建时+创建后</td>       <td>Tesla</td>     </tr>     <tr>       <td rowspan="2">GPU虚拟化</td>       <td>windows</td>       <td>指定镜像+创建后（部分系统）</td>       <td>GRID</td>       <td rowspan="2">sgn8ia、sgn7i-vws<br>vgn7i-vws、vgn6i-vws</td>     </tr>     <tr>       <td>linux</td>       <td>指定镜像+创建后（部分系统）</td>       <td>GRID</td>     </tr>   </tbody> </table>





### GPU计算-linux

- 适用的GPU实例：GPU卡为T4、A10、A30、A16、V100、P4、P100的实例（即所有Linux系统GPU计算型实例规格）
- 推荐安装的驱动：安装Tesla驱动来满足通用计算业务场景或图形加速/渲染场景。

| **推荐的驱动类型** | **驱动安装方式**                                             |
| ------------------ | ------------------------------------------------------------ |
| Tesla驱动          | **创建新实例时**：[创建GPU实例时自动安装或加载Tesla驱动](https://help.aliyun.com/zh/egs/user-guide/automatically-install-or-load-tesla-drivers-via-imgaes)GPU实例创建成功后首次启动时，将自动安装Tesla驱动。<br />**创建实例后**：[手动安装GPU驱动（Linux）](https://help.aliyun.com/zh/egs/user-guide/install-a-gpu-driver-on-a-gpu-accelerated-compute-optimized-linux-instance)或[通过YUM方式快速安装Tesla驱动（Alibaba Cloud Linux 3）](https://help.aliyun.com/zh/egs/user-guide/install-the-nvidia-tesla-driver-through-yum-alibaba-cloud-linux-3) |

### GPU计算-windows

- **通用计算业务场景**

  - 适用的GPU实例：GPU卡为T4、A10、A30、A16、V100、P4、P100的实例（即所有Windows系统GPU计算型实例规格）。

  - 推荐安装的驱动**：**

    | **推荐的驱动类型** | **驱动安装方式**                                             |
    | ------------------ | ------------------------------------------------------------ |
    | Tesla驱动          | **仅支持在创建实例后单独安装Tesla驱动**：[手动安装GPU驱动（Windows）](https://help.aliyun.com/zh/egs/user-guide/install-a-gpu-driver-on-a-gpu-accelerated-compute-optimized-windows-instance) |

- **图形加速/渲染场景**

  - 适用的GPU实例：GPU卡为T4、A10的实例（即**gn6i、gn7i、ebmgn6i、ebmgn7i**实例规格）。

  - 推荐安装的驱动**：**

    | **推荐的驱动类型**   | **驱动安装方式**                                             |
    | -------------------- | ------------------------------------------------------------ |
    | GRID驱动（15.2版本） | **创建新实例时**：[通过预装驱动的镜像加载GRID驱动](https://help.aliyun.com/zh/egs/user-guide/use-an-alibaba-cloud-marketplace-image-with-a-pre-installed-driver-to-load-a-grid-driver)<br />**说明**在创建GPU实例时，直接在云市场镜像选用已预装GRID驱动的免费镜像，在创建GPU实例的同时加载了GRID驱动。这些免费镜像带有已经激活License的GRID驱动，您无需再手动安装GRID驱动。<br />**创建实例后：**[通过云助手单独安装GRID驱动（Windows）](https://help.aliyun.com/zh/egs/user-guide/install-a-grid-driver-on-a-gpu-accelerated-compute-optimized-windows-instance-or-a-vgpu-accelerated-windows-instance) |

### GPU虚拟化-windows+linux

- 适用的GPU实例：GPU卡为T4、A10等的实例（即**vgn6i-vws、sgn7i-vws、vgn7i-vws**以及**sgn8ia**实例规格），更多信息，请参见[GPU虚拟化型（vgn/sgn系列）](https://help.aliyun.com/zh/egs/vgpu-accelerated-instance-families)。

- 推荐安装的驱动**：**安装GRID驱动来满足图形加速/渲染场景或通用计算业务场景。

  | **推荐的驱动类型**   | **驱动安装方式**                                             |
  | -------------------- | ------------------------------------------------------------ |
  | GRID驱动（13.5版本） | **创建新实例时：**[通过预装驱动的镜像加载GRID驱动](https://help.aliyun.com/zh/egs/user-guide/use-an-alibaba-cloud-marketplace-image-with-a-pre-installed-driver-to-load-a-grid-driver)<br />**说明**在创建GPU实例时，直接在云市场镜像中选用已预装GRID驱动的免费镜像，在创建GPU实例的同时加载了GRID驱动。这些免费镜像带有已经激活License的GRID驱动，您无需再手动安装GRID驱动。<br />**创建实例后****Windows：**[通过云助手单独安装GRID驱动（Windows）](https://help.aliyun.com/zh/egs/user-guide/install-a-grid-driver-on-a-gpu-accelerated-compute-optimized-windows-instance-or-a-vgpu-accelerated-windows-instance)**Linux：**[通过云助手单独安装GRID驱动（Linux）](https://help.aliyun.com/zh/egs/user-guide/use-cloud-assistant-to-automatically-install-and-upgrade-grid-drivers) |















