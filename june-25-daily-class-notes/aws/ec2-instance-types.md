# EC2 instance types

## Instance types <a href="#e62e" id="e62e"></a>

An EC2 instance is a virtual machine (VM) that runs in the AWS Cloud. When you launch an instance, you decide the virtual hardware configuration by choosing an instance type. The instance type that you choose determines the hardware of the host computer used for your instance. Each instance type offers different compute, memory, and storage capabilities, and is grouped into an instance family based on these capabilities.

**What to remember?**

* An **instance** is a VM.
* An **instance type** is the combination of virtual hardware components, such as CPU and memory, that make up the instance.
* Instance types are grouped together into **instance families**. Each instance family is optimized for specific types of use cases.
* Instance families have **sub-families**, which are grouped according to the combination of processer and storage used.
* A virtual central processing unit (**vCPU**) is a measure of processing ability. For most instance types, a vCPU represents one thread of the underling physical CPU core. For example, if an instance type has two CPU cores and two threads per core, it will have four vCPUs.

The AWS instances are currently categorized into five distinct families. To learn more, expand each of the following five categories.

## General purpose <a href="#id-112e" id="id-112e"></a>

**General purpose instances** provide a balance of compute, memory, and networking resources and can be used for a wide range of workloads. These instances are ideal for applications that use these resources in equal proportions, such as web servers and code repositories.

**Burstable instance options:** Many workloads are not busy all the time and do not require sustained CPU performance. Using a large instance for these low-to-moderate workloads leads to waste and unnecessary cost.

For these workloads you can take advantage of the low-cost burstable general purpose instances, which are the T family instances. A **burst** is when the activity on the instance exceeds normal operation for a short period; for example, when the workload temporarily spikes. The T instance family provides a baseline CPU performance with the ability to burst above the baseline at any time for as long as required. The T instances offer a balance of compute, memory, and network resources. They provide you with the most cost-effective way to run a broad spectrum of general purpose applications that have a low-to-moderate CPU usage.

For additional information, see [Burstable performance instances.](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/burstable-performance-instances.html)

The following diagram shows the icons for the general purpose family and sub-families as of this publication.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/0*QDuEQWqEuv2rbxIh.png" alt="" height="540" width="700"><figcaption></figcaption></figure>

## Compute optimized <a href="#id-55e8" id="id-55e8"></a>

Compute optimized instances are ideal for compute-bound applications that benefit from high-performance processors. Instances belonging to this family are well suited for compute-intensive operations, such as the following:

* Batch processing workloads
* Media transcoding
* High performance web servers
* High performance computing (HPC)
* Scientific modeling
* Dedicated gaming servers and ad server engines
* Machine learning (ML) inference

The following diagram shows the icons for the compute-optimized family and sub-families as of this publication.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/0*CSmCy4mZNFcxKRXw.png" alt="" height="499" width="700"><figcaption></figcaption></figure>

## Memory optimized <a href="#id-95f5" id="id-95f5"></a>

Memory optimized instances are designed to deliver fast performance for workloads that process large data sets in memory.

The following diagram shows the icons for the memory optimized family and sub-families as of this publication.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/0*C86ImZ_4mAc4UIX5.png" alt="" height="502" width="700"><figcaption></figcaption></figure>

## Storage optimized <a href="#c9d4" id="c9d4"></a>

Storage optimized instances are designed for workloads that require high, sequential read and write access to very large data sets on local storage. They are optimized to deliver tens of thousands of low-latency, random input/output (I/O) operations per second (IOPS) to applications.

The following diagram shows the icons for the storage optimized family and sub-families as of this publication.

<figure><img src="https://miro.medium.com/v2/resize:fit:1104/0*iGS6-KFB5iA0mfWZ.png" alt="" height="958" width="552"><figcaption></figcaption></figure>

## Accelerated computing <a href="#id-28f0" id="id-28f0"></a>

Accelerated computing instances use hardware accelerators, or co-processors, to perform some functions more efficiently than is possible in software running on CPUs. Examples of such functions include floating point number calculations, graphics processing, and data pattern matching. Accelerated computing instances facilitate more parallelism for higher throughput on compute-intensive workloads.

If you require high processing capability, you will benefit from using accelerated computing instances, which provide access to hardware-based compute accelerators such as graphics processing units (GPUs), field programmable gate arrays (FPGAs), or AWS Inferentia.

The following diagram shows the icons for the accelerated computing family and sub families as of this publication.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/0*K1eNOipyZ67K48ur.png" alt="" height="535" width="700"><figcaption></figcaption></figure>
