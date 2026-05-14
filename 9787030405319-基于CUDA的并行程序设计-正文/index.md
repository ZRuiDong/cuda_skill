# 《3165583C-8480-4Bf4-8A8C-2Ef7781770C5》技能集合索引

本技能集合聚焦于高性能计算、并行编程与异构系统优化，涵盖CUDA GPU编程、分布式计算、中间件架构、图像处理加速及任务调度等核心领域。内容深入探讨了从底层内存管理（如共享内存、纹理内存、固定内存）到高层系统设计（如CPU-GPU任务划分、分布式可扩展性分析）的关键技术，并结合遥感图像分割、SIFT特征提取等实际应用场景，提供完整的性能分析与优化方法论。

## Available Skills

| Skill | Description | Use When |
|-------|-------------|----------|
| [critical-section-mutual-exclusion](critical-section-mutual-exclusion/SKILL.md) | 确保多个并发进程对共享临界资源的互斥访问，防止数据竞争和不一致。 | 当多个进程需同时访问同一临界资源时，必须应用此技能以满足六项互斥原则。 |
| [cuda-3d-array-allocation-with-dimension-constraints](cuda-3d-array-allocation-with-dimension-constraints/SKILL.md) | 为纹理绑定数据、三维体数据或多通道图像分配支持纹理缓存的多维CUDA数组，并验证维度约束。 | 在需要调用cudaMalloc3DArray并确保宽度、高度、深度在有效范围内以成功分配GPU内存时使用。 |
| [cuda-pinned-host-memory-allocation](cuda-pinned-host-memory-allocation/SKILL.md) | 使用cudaMallocHost分配分页锁定主机内存以提升带宽和启用异步传输。 | 在需要高频或异步地在主机与GPU之间传输数据，如用于缓冲区或异步传输源/目标时使用。 |
| [cpu-gpu-task-partitioning](cpu-gpu-task-partitioning/SKILL.md) | 将程序划分为适合CPU执行的串行/控制密集部分与适合GPU执行的数据并行部分。 | 当面临高性能计算应用优化、并行程序设计或异构系统资源调度问题，且任务包含高计算密度、低分支复杂度的数据并行子任务时使用。 |
| [deadlock-four-conditions-analysis](deadlock-four-conditions-analysis/SKILL.md) | 分析系统是否同时满足死锁的四个必要条件（互斥、非预先占有、资源等待、循环等待）。 | 当需要诊断多线程程序、操作系统资源管理或并发进程中的死锁风险时使用。 |
| [cuda-block-size-and-sm-resource-constraint-analysis](cuda-block-size-and-sm-resource-constraint-analysis/SKILL.md) | 根据GPU SM资源规格和内核需求，计算每个SM可容纳的最大活动block数以指导block尺寸设计。 | 在设计或优化CUDA内核的block/grid配置，需确保高资源利用率和满足最小并发要求（至少2个活动block，6个warp）时使用。 |
| [gpu-linear-memory-allocation](gpu-linear-memory-allocation/SKILL.md) | 在CUDA上下文中为GPU设备分配未初始化的线性内存块。 | 当需要在GPU上动态申请指定字节数的线性存储空间（如用于数据传输或计算），且不涉及CUDA数组或主机内存时使用。 |
| [cuda-graphics-resource-access-optimization](cuda-graphics-resource-access-optimization/SKILL.md) | 为已注册的CUDA图形资源设置最优访问标志（只读、只写丢弃或默认）以减少同步开销和延迟。 | 当应用程序明确知道CUDA对图形资源的访问模式（如只读渲染结果或完全覆盖写入）时使用。 |
| [gpu-vs-distributed-cluster-parallel-selection](gpu-vs-distributed-cluster-parallel-selection/SKILL.md) | 在GPU并行与分布式集群并行之间进行技术选型。 | 当面临大规模计算任务、已知任务数据规模与通信模式，需由并行算法设计者或架构师做出合理架构选择时使用。 |
| [middleware-core-concept-and-application](middleware-core-concept-and-application/SKILL.md) | 定义中间件的核心概念、作用机制与适用场景。 | 当系统需在异构计算环境中实现应用互操作与资源共享，需判断是否引入中间件并理解其架构时使用。 |
| [deadlock-resolution-strategy](deadlock-resolution-strategy/SKILL.md) | 处理死锁风险或已发生的死锁，实施检测与恢复、避免或预防策略。 | 当检测到死锁条件或系统存在死锁隐患时使用。 |
| [cuda-pinned-memory-async-execution](cuda-pinned-memory-async-execution/SKILL.md) | 利用页锁定内存和CUDA流实现主机-设备数据传输与计算的重叠执行，隐藏传输延迟。 | 当需要提升吞吐量且存在可并行的数据传输与计算任务时使用。 |
| [parallel-algorithm-granularity-classification](parallel-algorithm-granularity-classification/SKILL.md) | 根据计算量与通信开销对并行算法的任务粒度进行分类（粗/中/细）。 | 当设计并行算法、优化GPU程序或制定任务调度策略时使用。 |
| [cuda-graphics-resource-interoperability-lifecycle](cuda-graphics-resource-interoperability-lifecycle/SKILL.md) | 管理OpenGL或Direct3D图形资源在CUDA中的注册、映射、访问与注销完整流程。 | 当应用程序需要在CUDA内核中直接读写图形API资源（如OpenGL缓冲区/纹理或Direct3D资源）时使用。 |
| [cpu-gpu-heterogeneous-system-advantages-assessment](cpu-gpu-heterogeneous-system-advantages-assessment/SKILL.md) | 评估CPU+GPU异构架构在计算能力互补性、存储带宽和能效性价比方面的优势。 | 当需为超级计算机、数据中心或能效敏感型系统选择架构，且应用场景包含大规模数据并行计算时使用。 |
| [gpu-heterogeneous-three-stage-execution](gpu-heterogeneous-three-stage-execution/SKILL.md) | 在CPU与GPU拥有独立内存空间的系统中，显式执行“数据传输→GPU计算→结果回传”三阶段流程。 | 当需要在GPU上执行计算任务且输入数据位于主机内存时，适用于CUDA、OpenCL等显式内存管理的GPU加速程序。 |
| [dooc-middleware-node-configuration](dooc-middleware-node-configuration/SKILL.md) | 配置D0oC中间件在计算节点、数据节点或混合节点上的组件部署规则。 | 当节点属于D0oC集群且需支持CUDA核外计算时，根据节点角色正确初始化中间件架构。 |
| [opengl-cuda-interop-device-binding](opengl-cuda-interop-device-binding/SKILL.md) | 执行OpenGL上下文与CUDA设备互操作的设备查询、绑定及资源注册标准化流程。 | 当应用程序需在GPU上共享OpenGL图形资源（如缓冲区或纹理）与CUDA计算时使用。 |
| [cuda-timing-and-synchronization](cuda-timing-and-synchronization/SKILL.md) | 精确测量CUDA内核函数、异步API调用或主机-设备数据传输的执行时间。 | 当需要对CUDA操作进行性能分析且要求时间测量准确可靠，需处理GPU异步执行与流同步机制时使用。 |
| [cuda-parallel-programming-curriculum-design](cuda-parallel-programming-curriculum-design/SKILL.md) | 根据学习者背景和目标应用领域，生成个性化的CUDA并行编程学习路径与内容组织方案。 | 当用户需要规划CUDA学习路线、设计并行编程课程或组织技术书籍结构时使用。 |
| [mimd-multiprocessor-system-classification](mimd-multiprocessor-system-classification/SKILL.md) | 根据内存组织方式、处理器数量和通信模型对MIMD多处理机系统进行分类并推荐子类型（SMP/DSM/MPP）。 | 当需要设计或评估高性能计算集群、异构计算系统或分布式内存架构时使用。 |
| [flynn-parallel-computer-classification](flynn-parallel-computer-classification/SKILL.md) | 根据指令流和数据流数量对并行计算机系统进行基础分类（SISD、SIMD、MISD、MIMD）。 | 当需要理解或选择并行计算架构、指导并行编程模型设计，或分析处理器架构类型时使用。 |
| [gpu-acceleration-ratio-prediction-for-remote-sensing-image-segmentation](gpu-acceleration-ratio-prediction-for-remote-sensing-image-segmentation/SKILL.md) | 预测CUDA并行实现的遥感图像水平集分割任务中基于图像尺寸和分块大小的GPU加速比。 | 当执行大规模遥感图像（≥5548×11036像素）的GPU并行处理且需评估性能收益时使用。 |
| [cuda-shared-memory-bank-conflict-optimization](cuda-shared-memory-bank-conflict-optimization/SKILL.md) | 识别并消除CUDA内核中共享存储器访问的bank冲突。 | 当共享存储器访问成为性能瓶颈，且使用共享存储器数组、特定线程索引和内存访问模式时使用。 |
| [cuda-2d-memory-allocation-with-pitch](cuda-2d-memory-allocation-with-pitch/SKILL.md) | 使用cudaMallocPitch在GPU上分配带对齐填充的二维显存以优化跨行访问和2D内存复制。 | 在需要高效处理图像或矩阵等二维数据结构、优化内存访问性能时使用，不适用于一维简单缓冲区。 |
| [simple-task-graph-determination](simple-task-graph-determination/SKILL.md) | 判定给定的有向无环任务图（DAG）是否为简单任务图。 | 当用户提供任务图G并需要判断其结构是否可通过代数因式分解简化时使用，适用于MIMD任务调度模型和并行算法表示。 |
| [out-of-core-computation-io-bottleneck-detection](out-of-core-computation-io-bottleneck-detection/SKILL.md) | 判定D0oC中间件环境下核外计算是否受I/O带宽限制。 | 当矩阵总大小超过集群可用内存总量且任务进入核外模式时，用于识别性能瓶颈并提出优化建议。 |
| [direct3d-cuda-interoperability-setup](direct3d-cuda-interoperability-setup/SKILL.md) | 配置Direct3D（9/10/11）与CUDA的互操作环境，包括设备绑定、资源注册及格式合规性验证。 | 当应用程序需要在同一个GPU上共享Direct3D渲染资源与CUDA计算时使用。 |
| [cuda-shared-memory-allocation](cuda-shared-memory-allocation/SKILL.md) | 为CUDA kernel正确声明和分配共享存储器，支持静态和动态两种方式。 | 当需要在kernel中使用共享存储器以提升线程间数据交换效率时使用。 |
| [cuda-graphics-interoperability-error-diagnosis](cuda-graphics-interoperability-error-diagnosis/SKILL.md) | 诊断CUDA图形互操作API调用失败的原因。 | 当调用如cudaGraphicsMapResources等图形互操作函数后，需立即检查cudaGetLastError()并解析错误字符串以定位问题时使用。 |
| [cuda-optimization-strategy-prioritization](cuda-optimization-strategy-prioritization/SKILL.md) | 提供按性能影响程度排序的CUDA程序性能优化策略。 | 当用户开始对已并行化的CUDA程序进行性能调优、且已识别主要性能瓶颈时使用。 |
| [deterministic-task-scheduling-model](deterministic-task-scheduling-model/SKILL.md) | 构建静态任务调度方案，已知所有任务的依赖关系和执行时间。 | 适用于MIMD系统、静态任务调度场景及并行算法实现，目标是最小化总执行时间。 |
| [dog-extrema-three-step-detection](dog-extrema-three-step-detection/SKILL.md) | 在SIFT特征提取中对DoG尺度空间执行三步极值检测以识别稳定关键点。 | 当需要从已构建的DoG图像金字塔中筛选出具有高对比度、非边缘且在空间与尺度上均为局部极值的候选关键点时使用。 |
| [cuda-image-processing-pipeline-optimization](cuda-image-processing-pipeline-optimization/SKILL.md) | 合并多个连续图像处理CUDA内核为单一内核，避免中间结果反复拷贝。 | 当处理灰度图像、已实现CUDA版本的imadjust和imerode函数、且流程中存在连续无数据依赖中断的操作时使用。 |
| [cuda-parallel-programming-fundamentals](cuda-parallel-programming-fundamentals/SKILL.md) | 提供CUDA并行程序的核心结构、线程与内存层次模型及基本开发流程。 | 当需要在NVIDIA GPU上实现计算密集型任务的并行加速时使用。 |
| [cuda-shared-memory-bank-conflict-analysis](cuda-shared-memory-bank-conflict-analysis/SKILL.md) | 判断CUDA共享存储器访问是否存在bank冲突并评估性能影响。 | 当多个线程在同一block内并发访问共享存储器时用于优化内存访问模式。 |
| [gpu-accelerated-sift-feature-matching](gpu-accelerated-sift-feature-matching/SKILL.md) | 对两组L2正规化的SIFT特征描述子执行GPU加速的双向最近邻匹配，结合距离阈值和NNDR准则筛选匹配对。 | 当需要在大规模图像特征集中高效、鲁棒地建立特征对应关系时使用。 |
| [cuda-based-three-level-image-tiling-for-level-set-segmentation](cuda-based-three-level-image-tiling-for-level-set-segmentation/SKILL.md) | 将大尺寸遥感图像或水平集分割任务按三级分块策略划分以适配CPU-GPU协同处理。 | 当处理大尺寸图像且系统具备CUDA支持时使用，确保显存高效利用并避免内存溢出。 |
| [parallel-speedup-evaluation](parallel-speedup-evaluation/SKILL.md) | 定义并计算两种并行加速比（单处理器基准与串行机基准）以量化性能提升。 | 当需要评估固定规模问题在多处理器系统上的并行效率时使用。 |
| [cuda-fdividef-precision-awareness](cuda-fdividef-precision-awareness/SKILL.md) | 识别在-prec-div=false模式下使用_fdividef处理大除数时的精度异常风险。 | 当涉及单精度浮点除法且关注数值正确性时使用。 |
| [cuda-performance-analysis-three-core-metrics](cuda-performance-analysis-three-core-metrics/SKILL.md) | 评估CUDA内核的带宽利用率、设备利用率和平均指令吞吐量以识别性能瓶颈。 | 当用户已通过Profiler或Nsight收集性能数据并需解读关键指标时使用。 |
| [cuda-performance-optimization-strategy](cuda-performance-optimization-strategy/SKILL.md) | 提供涵盖设备利用率、存储器吞吐量和指令吞吐量的CUDA程序性能优化策略。 | 当已有可运行的CUDA程序且需进一步提升性能时使用。 |
| [parallel-algorithm-time-complexity-and-work-analysis](parallel-algorithm-time-complexity-and-work-analysis/SKILL.md) | 计算并行算法在PRAM模型下的理论时间复杂度和总耗费（Work）。 | 当需要判断并行算法是否优于串行实现、分析加速比潜力或估算资源成本时使用。 |
| [sift-128d-descriptor-generation](sift-128d-descriptor-generation/SKILL.md) | 为已确定主方向的SIFT特征点生成128维旋转和亮度不变性描述子向量。 | 当用户提供具备有效位置、尺度和主方向的SIFT关键点，并需构建用于匹配或识别的标准化描述子时使用。 |
| [cuda-accelerated-matlab-image-processing](cuda-accelerated-matlab-image-processing/SKILL.md) | 提供将MATLAB中计算密集型图像处理算法通过CUDA加速的标准七步流程。 | 当用户需要显著提升MATLAB图像处理性能，且目标代码段具备并行化潜力时使用。 |
| [matlab-cuda-acceleration-methods](matlab-cuda-acceleration-methods/SKILL.md) | 帮助MATLAB用户根据硬件条件、预算和开发经验选择最适合的CUDA加速方法。 | 当需要在MATLAB中加速数值计算或图像处理任务且已配置CUDA驱动与Toolkit时使用。 |
| [sift-feature-orientation-calculation](sift-feature-orientation-calculation/SKILL.md) | 为SIFT特征点计算旋转不变性的主方向，使用36-bin梯度方向直方图结合高斯加权和平滑。 | 当需要为SIFT特征点分配主方向以实现旋转不变性描述子构建时使用。 |
| [cuda-imtophat-filter-three-stage](cuda-imtophat-filter-three-stage/SKILL.md) | 使用三阶段CUDA高帽滤波流程高效提取背景不均匀灰度图像中的亮细节。 | 当结构元素为圆盘形、输入为uint8格式、已绑定纹理内存并指定圆盘半径时使用。 |
| [cuda-texture-memory-usage](cuda-texture-memory-usage/SKILL.md) | 提供纹理存储器的完整使用流程，包括绑定、访问及解绑操作。 | 当需要对只读数据利用纹理缓存的硬件优化特性（如滤波、归一化坐标）时使用。 |
| [cuda-atomic-inc-dec-modulo-behavior](cuda-atomic-inc-dec-modulo-behavior/SKILL.md) | 使用atomicInc或atomicDec实现带上限的循环计数（模val+1行为）以确保线程安全。 | 当在CUDA内核中需对全局或共享内存中的32位无符号整数进行原子递增/递减，用于循环索引、环形缓冲区或同步计数器时使用。 |
| [detect-load-imbalance-in-distributed-computing](detect-load-imbalance-in-distributed-computing/SKILL.md) | 识别分布式计算中因任务划分不均导致的负载不均衡问题 | 当某节点在多轮迭代中持续早于其他节点完成计算任务时触发，适用于核外计算和CUDA中间件环境下的性能分析 |
| [distributed-computing-middleware-selection](distributed-computing-middleware-selection/SKILL.md) | 评估并选用面向分布式计算环境的中间件 | 当系统需处理海量数据、运行复杂算法，并部署于异构或同构分布式环境时使用；不适用于轻量级事务处理、消息传递或对实时性要求极高的系统 |
| [distributed-data-service-non-blocking-task-handling](distributed-data-service-non-blocking-task-handling/SKILL.md) | 实现非阻塞的外存查询或跨节点数据请求机制 | 当节点需处理数据请求但本地无所需数据时使用，适用于支持外存与网络通信的多节点协同系统，不适用于强一致性同步阻塞场景 |
| [subpixel-extremum-localization](subpixel-extremum-localization/SKILL.md) | 对候选极值点执行亚像素级精确定位 | 当subpixel_localization标志为真且候选点已通过初步极值检测时使用，适用于SIFT等高精度特征提取场景 |
| [io-compute-overlap-efficiency-evaluation](io-compute-overlap-efficiency-evaluation/SKILL.md) | 评估I/O操作与计算任务是否被有效重叠 | 当用户需要判断I/O是否成为性能瓶颈、或验证系统是否通过并行机制“掩盖”了I/O等待时使用，适用于异步I/O和并行计算环境 |
| [execution-unit-data-request-and-computation](execution-unit-data-request-and-computation/SKILL.md) | 执行标准化的数据请求、阻塞等待、资源调度与结果通知流程 | 当执行单元（CPU或GPU）接收到形如“X₁=A₁+B₁”的任务代码时使用，适用于异构计算环境中确保数据一致性与任务调度协同的场景 |
| [cuda-imadjust-grayscale-enhancement](cuda-imadjust-grayscale-enhancement/SKILL.md) | 使用CUDA并行实现灰度图像对比度增强 | 当需要在GPU上高效处理大批量uint8灰度图像以提升对比度，且已预计算LUT映射表时使用 |
| [datacutter-distributed-stream-processing](datacutter-distributed-stream-processing/SKILL.md) | 构建基于MPI的分布式数据流处理系统 | 当用户需部署支持多线程、GPU加速且依赖DataCutter中间件的流处理流水线时使用 |
| [dag-subgraph-data-aware-scheduling](dag-subgraph-data-aware-scheduling/SKILL.md) | 基于数据位置和节点负载优化DAG子图任务的初始调度 | 在系统启动完成外存扫描预处理后、首次分派DAG子图任务时使用，适用于需最小化跨节点数据传输并实现负载均衡的场景 |
| [laf-linear-algebra-immutable-programming](laf-linear-algebra-immutable-programming/SKILL.md) | 指导在线性代数操作中遵守不可变数据约束 | 当使用LAF框架调用线性代数原语处理矩阵、向量或标量时使用，以确保任务依赖清晰、支持DAG调度并避免数据一致性错误 |
| [distributed-global-array-chunk-management](distributed-global-array-chunk-management/SKILL.md) | 通过段页式分块机制管理超内存规模的全局数组 | 当需要处理核外计算任务、支持GPU加速且数据规模超过单节点内存限制时使用 |
| [dag-task-scheduling-efficiency-evaluation](dag-task-scheduling-efficiency-evaluation/SKILL.md) | 评估DAG任务创建与分配阶段的调度效率 | 当系统已完成任务创建与分配、各阶段时间可被精确测量，且使用LOBPCG算法与D0oC+LAF中间件架构时使用 |
| [distributed-system-scalability-bottleneck-analysis](distributed-system-scalability-bottleneck-analysis/SKILL.md) | 判断分布式线性求解器是否因通信开销出现性能拐点 | 当增加计算节点后总运行时间不再下降甚至上升，且通信量显著增长时使用 |
| [cuda-imerode-texture-memory-optimization](cuda-imerode-texture-memory-optimization/SKILL.md) | 使用CUDA纹理内存加速灰度图像的形态学腐蚀操作 | 当需要对灰度图像执行基于预定义结构元素（如十字形或圆盘形）的腐蚀，并希望利用纹理缓存提升二维空间局部性访问性能时使用 |
| [distributed-system-efficiency-calculation](distributed-system-efficiency-calculation/SKILL.md) | 计算分布式系统在固定问题规模下相对于2节点基准的资源利用效率 | 当需要评估多节点配置（n≥2）的绿色计算效果，并已知2节点和n节点的总运行时间时使用 |

## 快速导航

### CUDA 编程基础与内存管理
- cuda-parallel-programming-fundamentals
- gpu-linear-memory-allocation
- cuda-shared-memory-allocation
- cuda-2d-memory-allocation-with-pitch
- cuda-3d-array-allocation-with-dimension-constraints
- cuda-pinned-host-memory-allocation
- cuda-texture-memory-usage
- cuda-atomic-inc-dec-modulo-behavior

### CUDA 性能分析与优化
- cuda-performance-analysis-three-core-metrics
- cuda-performance-optimization-strategy
- cuda-optimization-strategy-prioritization
- cuda-block-size-and-sm-resource-constraint-analysis
- cuda-shared-memory-bank-conflict-analysis
- cuda-shared-memory-bank-conflict-optimization
- cuda-fdividef-precision-awareness
- io-compute-overlap-efficiency-evaluation
- execution-unit-data-request-and-computation

### GPU 与图形 API 互操作
- cuda-graphics-resource-interoperability-lifecycle
- cuda-graphics-resource-access-optimization
- cuda-graphics-interoperability-error-diagnosis
- opengl-cuda-interop-device-binding
- direct3d-cuda-interoperability-setup

### CPU-GPU 异构系统设计
- cpu-gpu-heterogeneous-system-advantages-assessment
- cpu-gpu-task-partitioning
- gpu-heterogeneous-three-stage-execution
- gpu-vs-distributed-cluster-parallel-selection

### 并行算法与复杂度分析
- parallel-algorithm-granularity-classification
- parallel-algorithm-time-complexity-and-work-analysis
- parallel-speedup-evaluation
- mimd-multiprocessor-system-classification
- flynn-parallel-computer-classification

### 任务调度与图计算
- simple-task-graph-determination
- dag-task-scheduling-efficiency-evaluation
- dag-subgraph-data-aware-scheduling
- deterministic-task-scheduling-model
- detect-load-imbalance-in-distributed-computing

### 分布式计算与中间件
- middleware-core-concept-and-application
- distributed-computing-middleware-selection
- dooc-middleware-node-configuration
- distributed-data-service-non-blocking-task-handling
- datacutter-distributed-stream-processing
- distributed-global-array-chunk-management
- distributed-system-scalability-bottleneck-analysis
- distributed-system-efficiency-calculation

### 图像处理与计算机视觉加速
- gpu-acceleration-ratio-prediction-for-remote-sensing-image-segmentation
- cuda-based-three-level-image-tiling-for-level-set-segmentation
- cuda-image-processing-pipeline-optimization
- sift-feature-orientation-calculation
- sift-128d-descriptor-generation
- gpu-accelerated-sift-feature-matching
- subpixel-extremum-localization
- dog-extrema-three-step-detection
- cuda-imtophat-filter-three-stage
- cuda-imerode-texture-memory-optimization
- cuda-imadjust-grayscale-enhancement

### MATLAB 与 CUDA 集成
- cuda-accelerated-matlab-image-processing
- matlab-cuda-acceleration-methods

### 死锁与同步机制
- critical-section-mutual-exclusion
- deadlock-four-conditions-analysis
- deadlock-resolution-strategy
- cuda-timing-and-synchronization

### 其他高级主题
- out-of-core-computation-io-bottleneck-detection
- cuda-parallel-programming-curriculum-design
- laf-linear-algebra-immutable-programming

## 使用方法

本技能集合可用于指导高性能计算项目开发、课程设计或系统优化。使用时，请结合具体场景引用对应技能名称。例如：

- **优化GPU图像处理流水线**：参考 `cuda-image-processing-pipeline-optimization` 和 `cuda-texture-memory-usage`。
- **诊断CUDA与OpenGL互操作错误**：查阅 `cuda-graphics-interoperability-error-diagnosis` 与 `opengl-cuda-interop-device-binding`。
- **评估分布式系统可扩展性瓶颈**：应用 `distributed-system-scalability-bottleneck-analysis` 和 `detect-load-imbalance-in-distributed-computing`。
- **设计MATLAB加速方案**：结合 `matlab-cuda-acceleration-methods` 与 `cuda-accelerated-matlab-image-processing`。

通过精准匹配技能名称，可快速定位所需技术细节与最佳实践。