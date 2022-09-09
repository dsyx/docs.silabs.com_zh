# AN1264: Using Open Thread with FreeRTOS™ (Rev. 0.2) <!-- omit in toc -->

- [1 开始](#1-开始)
- [2 main 函数和任务模型](#2-main-函数和任务模型)

---

<div>
    <details>
        <summary></summary>
        <p>
            The Silicon Labs OpenThread SDK provides support for running on top of FreeRTOS, a full-featured Real-Time Operating System (RTOS) for microcontrollers and small microprocessors. Support for FreeRTOS is integrated into Simplicity Studio.
        </p>
    </details>
    <p>
        Silicon Labs OpenThread SDK 为在 FreeRTOS 上运行提供了支持，FreeRTOS 是一个适用于微控制器和小型微处理器的功能齐全的实时操作系统（RTOS）。对 FreeRTOS 的支持已集成到 Simplicity Studio 中。
    </p>
</div>

<div>
    <details>
        <summary></summary>
        <p>
            FreeRTOS is supported on the EFR32MGxx family. For documentation on FreeRTOS, see <a href="https://www.freertos.org/">https://www.freertos.org/</a>.
        </p>
    </details>
    <p>
        EFR32MGxx 系列都支持 FreeRTOS。要查看 FreeRTOS 的文档，请访问 <a href="https://www.freertos.org/">https://www.freertos.org/</a>。
    </p>
</div>

# 1 开始

<div>
    <details>
        <summary></summary>
        <p>
            Integrating FreeRTOS into your application is simply a matter of installing the FreeRTOS component for the project in Simplicity Studio. The ot-ble-dmp sample application runs on FreeRTOS by default and you can add FreeRTOS to any OpenThread project. The ot-cli-ftd sample application is a good starting example.
        </p>
        <ol>
            <li>
                <p>In your project, double-click the <strong>.slcp</strong> file for the project in the Project Explorer to open the project window.</p>
            </li>
            <li>
                <p>Click the SOFTWARE COMPONENTS tab to see a complete list of Component categories.</p>
            </li>
            <li>
                <p>Find the FreeRTOS component located under RTOS &gt; FreeRTOS in the list of components.</p>
            </li>
            <li>
                <p>Select <strong>FreeRTOS</strong> and then click <strong>Install</strong>.</p>
                <p><img src="../images/AN1264/1.png"></p>
                <p>This component brings all the FreeRTOS kernel files into your project, along with some integration files and some additional components it depends on.</p>
            </li>
            <li>
                <p>Click <strong>View Dependencies</strong> to display the components.</p>
                <p>
                    One dependency is the CMSIS-RTOS2 component, which is an RTOS abstraction layer used by the integration files. Silicon Labs recommends that application developers use the FreeRTOS API directly rather than using the CMSIS-RTOS2 API.
                </p>
                <p>
                    Another dependency is the heap implementation used by FreeRTOS. FreeRTOS comes with five different heap implementations. Each appears as a component. By default, the FreeRTOS Heap 4 component is added to the project. You can change this by selecting a different heap component and clicking <strong>Install</strong>. For example, FreeRTOS HEAP 3 uses the system <code>malloc()</code> and <code>free()</code> implementation and is a common choice.
                </p>
            </li>
        </ol>
    </details>
    <p>
        要将 FreeRTOS 集成到您的应用程序中，只需在 Simplicity Studio 中为项目安装 FreeRTOS 组件即可。ot-ble-dmp 示例应用程序默认在 FreeRTOS 上运行，您可以将 FreeRTOS 添加到任何 OpenThread 项目。ot-cli-ftd 示例应用程序是一个很好的开始示例。
    </p>
    <ol>
        <li>
            <p>在您的项目中，在 Project Explorer 中双击该项目的 <strong>.slcp</strong> 文件以打开项目窗口。</p>
        </li>
        <li>
            <p>点击 SOFTWARE COMPONENTS 选项卡以查看组件类别的完整列表。</p>
        </li>
        <li>
            <p>在组件列表中的 RTOS &gt; FreeRTOS 下找到 FreeRTOS 组件。</p>
        </li>
        <li>
            <p>选择 <strong>FreeRTOS</strong>，然后点击 <strong>Install</strong>。</p>
            <p><img src="../images/AN1264/1.png"></p>
            <p>此组件会将所有 FreeRTOS 内核文件以及一些集成文件和它所依赖的一些附加组件带入到您的项目中。</p>
        </li>
        <li>
            <p>点击 <strong>View Dependencies</strong> 以显示该组件。</p>
            <p>
                其中一个依赖项是 CMSIS-RTOS2 组件，它是集成文件使用的 RTOS 抽象层。Silicon Labs 建议应用程序开发者直接使用 FreeRTOS API，而不是使用 CMSIS-RTOS2 API。
            </p>
            <p>
                另一个依赖项是 FreeRTOS 使用的堆实现。FreeRTOS 有五个不同的堆实现。每个都展示为一个组件。默认情况下，FreeRTOS Heap 4 组件会被添加到项目中。您可以通过选择不同的堆组件并点击 <strong>Install</strong> 来更改此设置。例如，FreeRTOS HEAP 3 使用系统的 <code>malloc()</code> 和 <code>free()</code> 实现，是一个常见的选择。
            </p>
        </li>
    </ol>
</div>

# 2 main 函数和任务模型

<div>
    <details>
        <summary></summary>
        <p>
            The FreeRTOS component is designed to be used along with the standard Silicon Labs main.c template. The standard main function works for both bare metal and kernel-based projects and takes care of all the required system initializations and task creation.
        </p>
    </details>
    <p>
        FreeRTOS 组件旨在与标准的 Silicon Labs main.c 模板一起使用。标准的 main 函数适用于裸机和基于内核的项目，并负责所有必需的系统初始化和任务创建。
    </p>
</div>

<div>
    <details>
        <summary></summary>
        <p>
            For OpenThread running on FreeRTOS, a single task is created that runs both the OpenThread stack and the application logic. It is not safe to call the OpenThread API from other tasks. 
        </p>
    </details>
    <p>
        对于在 FreeRTOS 上运行的 OpenThread，会创建单个任务，其同时运行 OpenThread 栈和应用程序逻辑。从其他任务调用 OpenThread API 是不安全的。
    </p>
</div>

<div>
    <details>
        <summary></summary>
        <p>
            In a bare metal OpenThread application, application logic is placed in the <code>app_process_action()</code> callback. Instead, when running on FreeRTOS, application logic is placed in the <code>sl_ot_rtos_application_tick()</code> callback.
        </p>
    </details>
    <p>
        在裸机的 OpenThread 应用程序中，应用程序逻辑放置在 <code>app_process_action()</code> 回调中。与之不同的是，在 FreeRTOS 上运行时，应用程序逻辑放置在 <code>sl_ot_rtos_application_tick()</code> 回调中。
    </p>
</div>
