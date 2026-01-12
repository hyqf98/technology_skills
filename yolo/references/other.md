# Yolo - Other

**Pages:** 30

---

## Reference for ultralytics/hub/__init__.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/hub/__init__/

**Contents:**
- Reference for ultralytics/hub/__init__.py
- function ultralytics.hub.login
- function ultralytics.hub.logout
- function ultralytics.hub.reset_model
- function ultralytics.hub.export_fmts_hub
- function ultralytics.hub.export_model
- function ultralytics.hub.get_export
- function ultralytics.hub.check_dataset

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/hub/__init__.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Log in to the Ultralytics HUB API using the provided API key.

The session is not stored; a new session is created when needed using the saved SETTINGS or the HUB_API_KEY environment variable if successfully authenticated.

Log out of Ultralytics HUB by removing the API key from the settings file.

Reset a trained model to an untrained state.

Return a list of HUB-supported export formats.

Export a model to a specified format for deployment via the Ultralytics HUB API.

Retrieve an exported model in the specified format from Ultralytics HUB using the model ID.

Check HUB dataset Zip file for errors before upload.

Download *.zip files from https://github.com/ultralytics/hub/tree/main/example_datasets i.e. https://github.com/ultralytics/hub/raw/main/example_datasets/coco8.zip for coco8.zip.

**Examples:**

Example 1 (typescript):
```typescript
def login(api_key: str | None = None, save: bool = True) -> bool
```

Example 2 (python):
```python
def login(api_key: str | None = None, save: bool = True) -> bool:
    """Log in to the Ultralytics HUB API using the provided API key.

    The session is not stored; a new session is created when needed using the saved SETTINGS or the HUB_API_KEY
    environment variable if successfully authenticated.

    Args:
        api_key (str, optional): API key to use for authentication. If not provided, it will be retrieved from SETTINGS
            or HUB_API_KEY environment variable.
        save (bool, optional): Whether to save the API key to SETTINGS if authentication is successful.

    Returns:
        (bool): True if authentication is successful, False otherwise.
    """
    checks.check_requirements("hub-sdk>=0.0.12")
    from hub_sdk import HUBClient

    api_key_url = f"{HUB_WEB_ROOT}/settings?tab=api+keys"  # set the redirect URL
    saved_key = SETTINGS.get("api_key")
    active_key = api_key or saved_key
    credentials = {"api_key": active_key} if active_key and active_key != "" else None  # set credentials

    client = HUBClient(credentials)  # initialize HUBClient

    if client.authenticated:
        # Successfully authenticated with HUB

        if save and client.api_key != saved_key:
            SETTINGS.update({"api_key": client.api_key})  # update settings with valid API key

        # Set message based on whether key was provided or retrieved from settings
        log_message = (
            "New authentication successful ✅" if client.api_key == api_key or not credentials else "Authenticated ✅"
        )
        LOGGER.info(f"{PREFIX}{log_message}")

        return True
    else:
        # Failed to authenticate with HUB
        LOGGER.info(f"{PREFIX}Get API key from {api_key_url} and then run 'yolo login API_KEY'")
        return False
```

Example 3 (python):
```python
def logout()
```

Example 4 (python):
```python
def logout():
    """Log out of Ultralytics HUB by removing the API key from the settings file."""
    SETTINGS["api_key"] = ""
    LOGGER.info(f"{PREFIX}logged out ✅. To log in again, use 'yolo login'.")
```

---

## 持续集成 (CI) - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/help/CI/

**Contents:**
- 持续集成 (CI)
- CI 操作
  - CI 结果
- 代码覆盖率
  - 与 codecov.io 集成
  - 覆盖率结果
- 常见问题
  - Ultralytics 中的持续集成 (CI) 是什么？
  - Ultralytics 如何检查文档和代码中的失效链接？
  - 为什么 CodeQL 分析对于 Ultralytics 的代码库如此重要？

持续集成 (CI) 是软件开发的一个重要方面，它涉及集成更改并自动测试它们。CI 使我们能够通过在开发过程中尽早且经常地发现问题来保持高质量的代码。在 Ultralytics，我们使用各种 CI 测试来确保代码库的质量和完整性。

下表显示了我们主要仓库的这些 CI 测试的状态：

每个徽章显示了相应 CI 测试上次运行的状态 main 相应存储库的分支。如果测试失败，徽章将显示“失败”状态；如果测试通过，则显示“通过”状态。

如果您发现有测试失败，如果您能在相应的存储库中通过 GitHub issue 报告它，那将非常有帮助。

请记住，成功的CI测试并不意味着一切都是完美的。 始终建议在部署或合并更改之前手动检查代码。

代码覆盖率是一个指标，表示在运行测试时执行的代码库的百分比。它可以帮助您了解测试对代码的覆盖程度，对于识别应用程序中未经测试的部分至关重要。较高的代码覆盖率通常与较低的错误可能性相关。但是，必须了解的是，代码覆盖率并不能保证没有缺陷。它仅指示代码的哪些部分已通过测试执行。

在 Ultralytics，我们将我们的代码仓库与 codecov.io 集成，这是一个流行的在线平台，用于测量和可视化代码覆盖率。Codecov 提供详细的见解、提交之间的覆盖率比较，以及直接在代码上的可视化覆盖层，指示哪些行被覆盖。

通过与 Codecov 集成，我们的目标是通过关注可能容易出错或需要进一步测试的领域来维护和提高代码质量。

要快速了解 的代码覆盖率状态 ultralytics Python ，我们已包含徽章和太阳放射状的可视化效果。 ultralytics 覆盖率结果。这些图表展示了测试覆盖的代码比例，为测试工作成效提供直观指标。完整详情请访问 Ultralytics 报告.

在下面的旭日形图中，最内圈是整个项目，远离中心的是文件夹，最后是单个文件。每个切片的大小和颜色分别代表语句的数量和覆盖率。

Ultralytics 中的持续集成 (CI) 涉及自动集成和测试代码更改，以确保高质量标准。我们的 CI 设置包括运行单元测试、代码规范检查和综合测试。此外，我们还执行Docker 部署、断链检查、针对安全漏洞的CodeQL 分析以及PyPI 发布，以打包和分发我们的软件。

Ultralytics 使用特定的 CI 操作来检查 markdown 和 HTML 文件中是否存在损坏的链接。这有助于通过扫描和识别死链接或损坏的链接来维护我们文档的完整性，确保用户始终可以访问准确且实时的资源。

CodeQL分析对于Ultralytics至关重要，因为它执行语义代码分析以查找潜在的安全漏洞并保持高质量标准。借助CodeQL，我们可以主动识别并缓解代码中的风险，从而帮助我们提供强大而安全的软件解决方案。

Ultralytics 使用 Docker 通过专门的 CI 操作来验证我们项目的部署。此过程确保我们的 Dockerfile 和相关脚本能够正常运行，从而实现一致且可重现的部署环境，这对于可扩展且可靠的 AI 解决方案至关重要。

自动化PyPI 发布确保我们的项目能够无误地打包和发布。这一步对于分发 Ultralytics 的 Python 包至关重要，使用户能够通过 Python 包索引 (PyPI) 轻松安装和使用我们的工具。

Ultralytics 通过与 Codecov 集成来衡量代码覆盖率，从而深入了解在测试期间执行了多少代码库。高代码覆盖率可以表明代码经过了充分的测试，有助于发现可能容易出现错误且未经测试的区域。可以通过我们主要存储库上显示的徽章或直接在 Codecov 上浏览详细的代码覆盖率指标。

---

## Ultralytics 安全策略 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/help/security/

**Contents:**
- Ultralytics 安全策略
- Snyk 扫描
- GitHub CodeQL 扫描
- GitHub Dependabot 警报
- GitHub Secret Scanning 警报
- 私有漏洞报告
- 常见问题
  - Ultralytics 实施了哪些安全措施来保护用户数据？
  - Ultralytics 如何使用 Snyk 进行安全扫描？
  - 什么是 CodeQL？它如何增强 Ultralytics 的安全性？

在Ultralytics，我们用户数据和系统的安全性至关重要。为确保我们开源项目的安全，我们已采取多项措施来检测和预防安全漏洞。

我们利用 Snyk 对 Ultralytics 存储库进行全面的安全扫描。Snyk 强大的扫描功能不仅限于依赖项检查，还会检查我们的代码和 Dockerfile 中存在的各种漏洞。通过主动识别和解决这些问题，我们确保为用户提供更高水平的安全性与可靠性。

我们的安全策略包括 GitHub 的 CodeQL 扫描。CodeQL 深入研究我们的代码库，通过分析代码的语义结构来识别复杂的漏洞，如 SQL 注入和 XSS。这种高级别的分析可确保及早发现和解决潜在的安全风险。

Dependabot 已集成到我们的工作流程中，用于监控已知漏洞的依赖项。当在我们的某个依赖项中发现漏洞时，Dependabot 会向我们发出警报，从而可以快速采取明智的补救措施。

我们采用 GitHub 秘密扫描警报来检测意外推送到我们仓库的敏感数据，例如凭据和私钥。这种早期检测机制有助于防止潜在的安全漏洞和数据泄露。

我们启用私有漏洞报告，允许用户谨慎地报告潜在的安全问题。 这种方法有助于负责任地披露，确保安全有效地处理漏洞。

如果您怀疑或发现我们任何存储库中存在安全漏洞，请立即告知我们。您可以通过我们的联系表单或 security@ultralytics.com 直接与我们联系。我们的安全团队将尽快调查并回复。

我们感谢您帮助我们确保所有 Ultralytics 开源项目的安全。

Ultralytics 采用了一项全面的安全策略，以保护用户数据和系统。关键措施包括：

这些工具确保主动识别和解决安全问题，从而增强整体系统安全性。欲了解更多详情，请查阅以上章节或联系安全团队咨询。

Ultralytics 利用 Snyk 对其存储库进行全面的安全扫描。Snyk 不仅限于基本的依赖项检查，还会检查代码和 Dockerfile 中是否存在各种漏洞。通过主动识别和解决潜在的安全问题，Snyk 有助于确保 Ultralytics 的开源项目保持安全可靠。

要查看 Snyk 徽章并了解有关其部署的更多信息，请查看Snyk 扫描部分。

CodeQL 是一种安全分析工具，通过 GitHub 集成到 Ultralytics 的工作流程中。它深入研究代码库以识别复杂的漏洞，如 SQL 注入和跨站脚本 (XSS)。CodeQL 分析代码的语义结构以提供高级别的安全性，确保及早发现和缓解潜在风险。

有关 CodeQL 使用方式的更多信息，请访问 GitHub CodeQL 扫描部分。

Dependabot 是一种自动化工具，用于监控和管理已知漏洞的依赖项。当 Dependabot 在 Ultralytics 项目依赖项中检测到漏洞时，它会发送警报，使团队能够快速解决和缓解问题。这确保了依赖项的安全和最新，从而最大限度地降低了潜在的安全风险。

有关更多详细信息，请浏览GitHub Dependabot Alerts 部分。

Ultralytics 鼓励用户通过私有渠道报告潜在的安全问题。用户可以通过联系表 discreetly 或发送电子邮件至 security@ultralytics.com 来报告漏洞。这确保了负责任的披露，并允许安全团队安全有效地调查和解决漏洞。

有关私有漏洞报告的更多信息，请参阅私有漏洞报告部分。

---

## 建设中 🏗️🌟 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/hub/api/

**Contents:**
- 建设中 🏗️🌟
- 即将推出的令人兴奋的新功能 🎉
- 保持更新 🚧
- 我们重视您的意见 🗣️
- 感谢社区！🌍

欢迎来到 Ultralytics “正在建设中”页面！我们正在努力开发下一代 AI 和 ML 创新技术。此页面是我们将与您分享的激动人心的更新和新功能的预告！

此页面是您获取最新集成更新和功能发布的首选资源。通过以下方式保持联系：

通过我们的官方联系表单分享您的想法、反馈和集成请求，帮助塑造 Ultralytics HUB 的未来。

您的贡献和持续支持推动我们不断突破人工智能创新的界限。敬请关注——激动人心的时刻即将到来！

对即将到来的功能感到兴奋吗？请收藏此页面，并查看我们的快速入门指南，以便在等待的同时开始使用我们当前的工具。准备好与Ultralytics一起踏上变革性的AI和ML之旅吧！🛠️🤖

---

## Ultralytics 、健康与安全（EHS）政策

**URL:** https://docs.ultralytics.com/zh/help/environmental-health-safety/

**Contents:**
- Ultralytics 、健康与安全（EHS）政策
- 政策原则
- 实施措施
- 常见问题
  - Ultralytics 的环境、健康和安全 (EHS) 政策是什么？
  - Ultralytics 如何确保符合 EHS 法规？
  - 为什么持续改进是 Ultralytics EHS 政策中的关键原则？
  - 在 Ultralytics 实施 EHS 政策时，员工的角色和职责是什么？
  - Ultralytics 的 EHS 政策如何处理应急准备和响应？
  - Ultralytics 如何与其利益相关者就其 EHS 表现进行沟通？

在 Ultralytics，我们认识到公司的长期成功不仅依赖于我们提供的产品和服务，还依赖于我们开展业务的方式。我们致力于确保我们的员工、利益相关者和环境的安全和福祉，并将不断努力减轻我们对环境的影响，同时促进健康和安全。

合规性: 我们将遵守所有适用的与EHS相关的法律、法规和标准，并且我们将努力在可能的情况下超越这些标准。

预防：我们将通过实施风险管理措施并确保我们所有的运营和程序都是安全的，来努力预防事故、伤害和环境损害。

持续改进: 我们将通过设定可衡量的目标、监控我们的绩效、审计我们的运营以及根据需要修订我们的政策和程序来不断提高我们的 EHS 绩效。

沟通 (Communication): 我们将公开沟通我们的 EHS 绩效，并将与利益相关者沟通，以了解和解决他们的疑虑和期望。

教育和培训: 我们将对员工和承包商进行适当的 EHS 程序和实践方面的教育和培训。

责任和问责: 在Ultralytics工作或与Ultralytics合作的每位员工和承包商都有责任遵守本政策。管理人员和主管有责任确保在其控制范围内实施本政策。

风险管理: 我们将识别、评估和管理与我们的运营和活动相关的EHS风险，以防止事故、伤害和环境损害。

资源分配: 我们将分配必要的资源，以确保有效实施我们的EHS政策，包括必要的设备、人员和培训。

应急准备和响应: 我们将制定、维护和测试应急准备和响应计划，以确保我们能够有效地应对 EHS 事件。

监控和审查: 我们将定期监控和审查我们的 EHS 绩效，以寻找改进的机会，并确保我们实现目标。

此政策反映了我们对最大限度减少环境足迹、确保员工安全和福祉以及不断提高绩效的承诺。

请记住，有效实施 EHS 政策需要 Ultralytics 所有员工或与 Ultralytics 合作的人员的参与和承诺。我们鼓励您对自身和他人的安全负责，并照顾我们生活和工作的环境。

Ultralytics 的环境、健康和安全 (EHS) 政策是一个综合框架，旨在确保员工、利益相关者和环境的安全与福祉。它强调遵守相关法律，通过风险管理预防事故，通过可衡量的目标持续改进，公开沟通以及对员工进行教育和培训。通过遵循这些原则，Ultralytics 旨在最大限度地减少其环境足迹并促进可持续实践。 了解更多关于 Ultralytics 对 EHS 的承诺。

Ultralytics 通过遵守所有适用的法律、法规和标准来确保符合 EHS 规章。公司不仅努力满足这些要求，而且经常通过实施严格的内部政策来超越它们。会定期进行审计、监控和审查，以确保持续合规。管理人员和主管也负责确保在其控制范围内维持这些标准。有关更多详细信息，请参阅文档页面上的“政策原则”部分。

持续改进是 Ultralytics 的 EHS 政策的关键，因为它确保公司不断提高其在环境、健康和安全领域的绩效。通过设定可衡量的目标、监控绩效以及根据需要修订政策和程序，Ultralytics 可以适应新的挑战并优化其流程。这种方法不仅可以降低风险，还可以展示 Ultralytics 对可持续性和卓越性的承诺。有关持续改进的实际示例，请查看实施措施部分。

Ultralytics 的每位员工和承包商都有责任遵守 EHS 政策。这包括遵守安全协议、参加必要的培训，并对自身和他人的安全承担个人责任。管理人员和主管还负有额外的责任，即确保 EHS 政策在其控制范围内得到有效实施，这涉及风险评估和资源分配。有关责任和义务的更多信息，请参阅实施措施部分。

Ultralytics 通过制定、维护和定期测试应急计划来有效应对潜在的 EHS 事件，从而处理应急准备和响应。这些计划确保公司能够迅速有效地做出响应，从而最大限度地减少对员工、环境和财产的危害。会定期进行培训和演习，以使响应团队为各种紧急情况做好准备。有关更多背景信息，请参阅应急准备和响应措施。

Ultralytics 通过分享相关信息并解决任何问题或预期，与利益相关者开诚地沟通关于其 EHS 绩效。这种参与包括定期报告 EHS 活动、绩效指标和改进举措。还鼓励利益相关者提供反馈，这有助于 Ultralytics 不断完善其政策和实践。请在通信原则部分中了解有关这项承诺的更多信息。

---

## Reference for ultralytics/solutions/parking_management.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/parking_management/

**Contents:**
- Reference for ultralytics/solutions/parking_management.py
- class ultralytics.solutions.parking_management.ParkingPtsSelection
  - method ultralytics.solutions.parking_management.ParkingPtsSelection.draw_box
  - method ultralytics.solutions.parking_management.ParkingPtsSelection.initialize_properties
  - method ultralytics.solutions.parking_management.ParkingPtsSelection.on_canvas_click
  - method ultralytics.solutions.parking_management.ParkingPtsSelection.redraw_canvas
  - method ultralytics.solutions.parking_management.ParkingPtsSelection.remove_last_bounding_box
  - method ultralytics.solutions.parking_management.ParkingPtsSelection.save_to_json
  - method ultralytics.solutions.parking_management.ParkingPtsSelection.upload_image
- class ultralytics.solutions.parking_management.ParkingManagement

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/parking_management.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class for selecting and managing parking zone points on images using a Tkinter-based UI.

This class provides functionality to upload an image, select points to define parking zones, and save the selected points to a JSON file. It uses Tkinter for the graphical user interface.

Draw a bounding box on the canvas using the provided coordinates.

Initialize properties for image, canvas, bounding boxes, and dimensions.

Handle mouse clicks to add points for bounding boxes on the canvas.

Redraw the canvas with the image and all bounding boxes.

Remove the last bounding box from the list and redraw the canvas.

Save the selected parking zone points to a JSON file with scaled coordinates.

Upload and display an image on the canvas, resizing it to fit within specified dimensions.

Manages parking occupancy and availability using YOLO model for real-time monitoring and visualization.

This class extends BaseSolution to provide functionality for parking lot management, including detection of occupied spaces, visualization of parking regions, and display of occupancy statistics.

Process the input image for parking lot management and visualization.

This function analyzes the input image, extracts tracks, and determines the occupancy status of parking regions defined in the JSON file. It annotates the image with occupied and available parking spots, and updates the parking information.

**Examples:**

Example 1 (rust):
```rust
ParkingPtsSelection(self) -> None
```

Example 2 (sql):
```sql
>>> parking_selector = ParkingPtsSelection()
>>> # Use the GUI to upload an image, select parking zones, and save the data
```

Example 3 (python):
```python
class ParkingPtsSelection:
    """A class for selecting and managing parking zone points on images using a Tkinter-based UI.

    This class provides functionality to upload an image, select points to define parking zones, and save the selected
    points to a JSON file. It uses Tkinter for the graphical user interface.

    Attributes:
        tk (module): The Tkinter module for GUI operations.
        filedialog (module): Tkinter's filedialog module for file selection operations.
        messagebox (module): Tkinter's messagebox module for displaying message boxes.
        master (tk.Tk): The main Tkinter window.
        canvas (tk.Canvas): The canvas widget for displaying the image and drawing bounding boxes.
        image (PIL.Image.Image): The uploaded image.
        canvas_image (ImageTk.PhotoImage): The image displayed on the canvas.
        rg_data (list[list[tuple[int, int]]]): List of bounding boxes, each defined by 4 points.
        current_box (list[tuple[int, int]]): Temporary storage for the points of the current bounding box.
        imgw (int): Original width of the uploaded image.
        imgh (int): Original height of the uploaded image.
        canvas_max_width (int): Maximum width of the canvas.
        canvas_max_height (int): Maximum height of the canvas.

    Methods:
        initialize_properties: Initialize properties for image, canvas, bounding boxes, and dimensions.
        upload_image: Upload and display an image on the canvas, resizing it to fit within specified dimensions.
        on_canvas_click: Handle mouse clicks to add points for bounding boxes on the canvas.
        draw_box: Draw a bounding box on the canvas using the provided coordinates.
        remove_last_bounding_box: Remove the last bounding box from the list and redraw the canvas.
        redraw_canvas: Redraw the canvas with the image and all bounding boxes.
        save_to_json: Save the selected parking zone points to a JSON file with scaled coordinates.

    Examples:
        >>> parking_selector = ParkingPtsSelection()
        >>> # Use the GUI to upload an image, select parking zones, and save the data
    """

    def __init__(self) -> None:
        """Initialize the ParkingPtsSelection class, setting up UI and properties for parking zone point selection."""
        try:  # Check if tkinter is installed
            import tkinter as tk
            from tkinter import filedialog, messagebox
        except ImportError:  # Display error with recommendations
            import platform

            install_cmd = {
                "Linux": "sudo apt install python3-tk (Debian/Ubuntu) | sudo dnf install python3-tkinter (Fedora) | "
                "sudo pacman -S tk (Arch)",
                "Windows": "reinstall Python and enable the checkbox `tcl/tk and IDLE` on **Optional Features** during installation",
                "Darwin": "reinstall Python from https://www.python.org/downloads/macos/ or `brew install python-tk`",
            }.get(platform.system(), "Unknown OS. Check your Python installation.")

            LOGGER.warning(f" Tkinter is not configured or supported. Potential fix: {install_cmd}")
            return

        if not check_imshow(warn=True):
            return

        self.tk, self.filedialog, self.messagebox = tk, filedialog, messagebox
        self.master = self.tk.Tk()  # Reference to the main application window
        self.master.title("Ultralytics Parking Zones Points Selector")
        self.master.resizable(False, False)

        self.canvas = self.tk.Canvas(self.master, bg="white")  # Canvas widget for displaying images
        self.canvas.pack(side=self.tk.BOTTOM)

        self.image = None  # Variable to store the loaded image
        self.canvas_image = None  # Reference to the image displayed on the canvas
        self.canvas_max_width = None  # Maximum allowed width for the canvas
        self.canvas_max_height = None  # Maximum allowed height for the canvas
        self.rg_data = None  # Data for region annotation management
        self.current_box = None  # Stores the currently selected bounding box
        self.imgh = None  # Height of the current image
        self.imgw = None  # Width of the current image

        # Button frame with buttons
        button_frame = self.tk.Frame(self.master)
        button_frame.pack(side=self.tk.TOP)

        for text, cmd in [
            ("Upload Image", self.upload_image),
            ("Remove Last Bounding Box", self.remove_last_bounding_box),
            ("Save", self.save_to_json),
        ]:
            self.tk.Button(button_frame, text=text, command=cmd).pack(side=self.tk.LEFT)

        self.initialize_properties()
        self.master.mainloop()
```

Example 4 (python):
```python
def draw_box(self, box: list[tuple[int, int]]) -> None
```

---

## Reference for hub_sdk/base/auth.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/base/auth/

**Contents:**
- Reference for hub_sdk/base/auth.py
- class hub_sdk.base.auth.Auth
  - method hub_sdk.base.auth.Auth.authenticate
  - method hub_sdk.base.auth.Auth.authorize
  - method hub_sdk.base.auth.Auth.get_auth_header
  - method hub_sdk.base.auth.Auth.get_state
  - method hub_sdk.base.auth.Auth.set_api_key

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/base/auth.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Represents an authentication manager for Ultralytics Hub API.

This class handles authentication using either an API key or ID token, providing methods to authenticate, authorize, and manage authentication state.

Attempt to authenticate with the server using either id_token or API key.

Makes a POST request to the authentication endpoint with the appropriate authentication header. Handles connection errors and request exceptions.

Authorize the user by obtaining an idToken through a POST request with email and password.

Makes a request to the Firebase authentication URL with the provided credentials. Handles connection errors and request exceptions.

Get the authentication header for making API requests.

Creates the appropriate header based on whether an ID token or API key is available.

Get the authentication state.

Set the API key for authentication.

**Examples:**

Example 1 (python):
```python
class Auth:
    """Represents an authentication manager for Ultralytics Hub API.

    This class handles authentication using either an API key or ID token, providing methods to authenticate, authorize,
    and manage authentication state.

    Attributes:
        api_key (str | None): The API key used for authentication with the Hub API.
        id_token (str | None): The authentication token received after successful authorization.

    Methods:
        authenticate: Attempt to authenticate with the server using either id_token or API key.
        get_auth_header: Get the authentication header for making API requests.
        get_state: Get the authentication state.
        set_api_key: Set the API key for authentication.
        authorize: Authorize the user by obtaining an idToken through email and password.
    """

    def __init__(self):
        """Initialize the Auth class with default authentication settings."""
        self.api_key = None
        self.id_token = None
```

Example 2 (python):
```python
def authenticate(self) -> bool
```

Example 3 (python):
```python
def authenticate(self) -> bool:
    """Attempt to authenticate with the server using either id_token or API key.

    Makes a POST request to the authentication endpoint with the appropriate authentication header.
    Handles connection errors and request exceptions.

    Returns:
        (bool): True if authentication is successful, False otherwise.

    Raises:
        ConnectionError: If authentication fails or user has not authenticated locally.
    """
    try:
        if header := self.get_auth_header():
            r = requests.post(f"{HUB_API_ROOT}/v1/auth", headers=header)
            if not r.json().get("success", False):
                raise ConnectionError("Unable to authenticate.")
            return True
        raise ConnectionError("User has not authenticated locally.")
    except ConnectionError:
        logger.warning(f"{PREFIX} Invalid API key ⚠️")
    except requests.exceptions.RequestException as e:
        status_code = e.response.status_code if hasattr(e, "response") else None
        error_msg = ErrorHandler(status_code).handle()
        logger.warning(f"{PREFIX} {error_msg}")

    self.id_token = self.api_key = False  # reset invalid credentials
    return False
```

Example 4 (python):
```python
def authorize(self, email: str, password: str) -> bool
```

---

## Reference for ultralytics/solutions/heatmap.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/solutions/heatmap/

**Contents:**
- Reference for ultralytics/solutions/heatmap.py
- class ultralytics.solutions.heatmap.Heatmap
  - method ultralytics.solutions.heatmap.Heatmap.heatmap_effect
  - method ultralytics.solutions.heatmap.Heatmap.process

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/heatmap.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class to draw heatmaps in real-time video streams based on object tracks.

This class extends the ObjectCounter class to generate and visualize heatmaps of object movements in video streams. It uses tracked object positions to create a cumulative heatmap effect over time.

Efficiently calculate heatmap area and effect location for applying colormap.

Generate heatmap for each frame using Ultralytics tracking.

**Examples:**

Example 1 (rust):
```rust
Heatmap(self, **kwargs: Any) -> None
```

Example 2 (sql):
```sql
>>> from ultralytics.solutions import Heatmap
>>> heatmap = Heatmap(model="yolo11n.pt", colormap=cv2.COLORMAP_JET)
>>> frame = cv2.imread("frame.jpg")
>>> processed_frame = heatmap.process(frame)
```

Example 3 (python):
```python
class Heatmap(ObjectCounter):
    """A class to draw heatmaps in real-time video streams based on object tracks.

    This class extends the ObjectCounter class to generate and visualize heatmaps of object movements in video
    streams. It uses tracked object positions to create a cumulative heatmap effect over time.

    Attributes:
        initialized (bool): Flag indicating whether the heatmap has been initialized.
        colormap (int): OpenCV colormap used for heatmap visualization.
        heatmap (np.ndarray): Array storing the cumulative heatmap data.
        annotator (SolutionAnnotator): Object for drawing annotations on the image.

    Methods:
        heatmap_effect: Calculate and update the heatmap effect for a given bounding box.
        process: Generate and apply the heatmap effect to each frame.

    Examples:
        >>> from ultralytics.solutions import Heatmap
        >>> heatmap = Heatmap(model="yolo11n.pt", colormap=cv2.COLORMAP_JET)
        >>> frame = cv2.imread("frame.jpg")
        >>> processed_frame = heatmap.process(frame)
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the Heatmap class for real-time video stream heatmap generation based on object tracks.

        Args:
            **kwargs (Any): Keyword arguments passed to the parent ObjectCounter class.
        """
        super().__init__(**kwargs)

        self.initialized = False  # Flag for heatmap initialization
        if self.region is not None:  # Check if user provided the region coordinates
            self.initialize_region()

        # Store colormap
        self.colormap = self.CFG["colormap"]
        self.heatmap = None
```

Example 4 (python):
```python
def heatmap_effect(self, box: list[float]) -> None
```

---

## Reference for hub_sdk/base/server_clients.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/base/server_clients/

**Contents:**
- Reference for hub_sdk/base/server_clients.py
- class hub_sdk.base.server_clients.ModelUpload
  - method hub_sdk.base.server_clients.ModelUpload._handle_signal
  - method hub_sdk.base.server_clients.ModelUpload._register_signal_handlers
  - method hub_sdk.base.server_clients.ModelUpload._start_heartbeats
  - method hub_sdk.base.server_clients.ModelUpload._stop_heartbeats
  - method hub_sdk.base.server_clients.ModelUpload.export
  - method hub_sdk.base.server_clients.ModelUpload.predict
  - method hub_sdk.base.server_clients.ModelUpload.upload_metrics
  - method hub_sdk.base.server_clients.ModelUpload.upload_model

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/base/server_clients.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Manages uploading and exporting model files and metrics to Ultralytics HUB and heartbeat updates.

This class handles the communication with Ultralytics HUB API for model-related operations including uploading model checkpoints, metrics, exporting models to different formats, and maintaining heartbeat connections to track model training status.

Handle kill signals and prevent heartbeats from being sent on Colab after termination.

Register signal handlers for SIGTERM and SIGINT signals to gracefully handle termination.

Begin a threaded heartbeat loop to report the agent's status to Ultralytics HUB.

This method initiates a threaded loop that periodically sends heartbeats to Ultralytics HUB to report the status of the agent. Heartbeats are sent at regular intervals as defined in the 'rate_limits' dictionary.

Stop the threaded heartbeat loop.

This method stops the threaded loop responsible for sending heartbeats to Ultralytics HUB. It sets the 'alive' flag to False, which will cause the loop in '_start_heartbeats' to exit.

Export a model to a specific format.

Perform a prediction using the specified image and configuration.

Upload metrics data for a specific model.

Upload a model checkpoint to Ultralytics HUB.

Handle project file uploads to Ultralytics HUB via API requests.

This class manages the uploading of project-related files to Ultralytics HUB, providing methods to handle image uploads for projects.

Upload a project image to the hub.

Manages uploading dataset files to Ultralytics HUB via API requests.

This class handles the uploading of dataset files to Ultralytics HUB, providing methods to manage dataset uploads.

Upload a dataset file to the hub.

Check if the current script is running inside a Google Colab notebook.

**Examples:**

Example 1 (unknown):
```unknown
ModelUpload(self, headers)
```

Example 2 (python):
```python
class ModelUpload(APIClient):
    """Manages uploading and exporting model files and metrics to Ultralytics HUB and heartbeat updates.

    This class handles the communication with Ultralytics HUB API for model-related operations including uploading model
    checkpoints, metrics, exporting models to different formats, and maintaining heartbeat connections to track model
    training status.

    Attributes:
        name (str): Identifier for the model upload instance.
        alive (bool): Flag indicating if the heartbeat thread should continue running.
        agent_id (str): Unique identifier for the agent sending heartbeats.
        rate_limits (Dict): Dictionary containing rate limits for different API operations in seconds.
    """

    def __init__(self, headers):
        """Initialize ModelUpload with API client configuration.

        Args:
            headers (Dict): HTTP headers to use for API requests.
        """
        super().__init__(f"{HUB_API_ROOT}/v1/models", headers)
        self.name = "model"
        self.alive = True
        self.agent_id = None
        self.rate_limits = {"metrics": 3.0, "ckpt": 900.0, "heartbeat": 300.0}
```

Example 3 (python):
```python
def _handle_signal(self, signum: int, frame: Any) -> None
```

Example 4 (python):
```python
def _handle_signal(self, signum: int, frame: Any) -> None:
    """Handle kill signals and prevent heartbeats from being sent on Colab after termination.

    Args:
        signum (int): Signal number.
        frame (Any): The current stack frame (not used in this method).
    """
    self.logger.debug("Kill signal received!")
    self._stop_heartbeats()
    sys.exit(signum)
```

---

## 项目 - Ultralytics HUB-SDK 操作 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/hub/sdk/project/

**Contents:**
- 项目 - Ultralytics HUB-SDK 操作
- 通过 ID 获取项目
- 创建新项目
- 更新现有项目
- 删除项目
- 列出和导航项目
- 评论

欢迎阅读 Ultralytics HUB-SDK 文档！本指南将引导您了解使用 HUB-SDK 管理机器学习项目的要点。我们涵盖了从创建新项目、更新现有项目到浏览项目列表的所有内容，并提供了易于理解的 python 代码片段。我们的目标是提供无缝且直接的体验，让您能够专注于构建和部署卓越的机器学习模型。让我们开始吧 🏊！

要检索 Ultralytics 平台上托管的项目并查看其详细信息或进行更改，请通过其唯一 ID 获取它。将 ID 传递给 client.project 函数，如下面的代码片段所示：

有关更多详细信息，请参见 参考 hub_sdk/modules/projects.py.

通过以下方式开始一个新的机器学习项目 创建一个项目 在 Ultralytics HUB 中。以下 python 代码概述了如何定义项目详细信息（在本例中为其名称）并使用以下代码创建项目 create_project 方法：

通过指定项目ID和新详细信息，轻松更新项目的元数据。这可能包括名称更改、描述更新或其他可修改的属性。使用以下代码片段执行这些更改：

要从 Ultralytics 平台删除项目，请使用 delete 项目对象上的 方法。以下代码段指导您使用其 ID 删除项目：

通过获取具有所需页面大小的列表，浏览您的项目或在 Ultralytics 上探索 公共项目。下面的代码片段演示了如何查看当前页面结果、导航到下一页以及返回到上一页：

恭喜！您现在可以轻松地在 Ultralytics HUB 上管理您的机器学习项目。尝试这些操作，以提高您的 ML 工作的组织性和效率。如有任何问题或需要进一步帮助，请随时联系我们的社区。祝您编码愉快！🚀

**Examples:**

Example 1 (python):
```python
from hub_sdk import HUBClient

credentials = {"api_key": "<YOUR-API-KEY>"}  # Replace with your API key
client = HUBClient(credentials)

project = client.project("<Project ID>")  # Replace '<Project ID>' with your actual project ID
print(project.data)  # Displays the project's data
```

Example 2 (json):
```json
from hub_sdk import HUBClient

credentials = {"api_key": "<YOUR-API-KEY>"}  # Replace with your API key
client = HUBClient(credentials)

data = {"meta": {"name": "my project"}}  # Define the project name
project = client.project()  # Initialize a project instance
project.create_project(data)  # Create the new project with the specified data
```

Example 3 (sql):
```sql
from hub_sdk import HUBClient

credentials = {"api_key": "<YOUR-API-KEY>"}  # Replace with your API key
client = HUBClient(credentials)

project = client.project("<Project ID>")  # Replace with your actual project ID
project.update({"meta": {"name": "Project name update"}})  # Update the project's name or other metadata
```

Example 4 (json):
```json
from hub_sdk import HUBClient

credentials = {"api_key": "<YOUR-API-KEY>"}  # Replace with your API key
client = HUBClient(credentials)

project = client.project("<Project ID>")  # Replace with the project ID to delete
project.delete()  # Permanently deletes the project
```

---

## Reference for ultralytics/nn/tasks.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/nn/tasks/

**Contents:**
- Reference for ultralytics/nn/tasks.py
- class ultralytics.nn.tasks.BaseModel
  - method ultralytics.nn.tasks.BaseModel._apply
  - method ultralytics.nn.tasks.BaseModel._predict_augment
  - method ultralytics.nn.tasks.BaseModel._predict_once
  - method ultralytics.nn.tasks.BaseModel._profile_one_layer
  - method ultralytics.nn.tasks.BaseModel.forward
  - method ultralytics.nn.tasks.BaseModel.fuse
  - method ultralytics.nn.tasks.BaseModel.info
  - method ultralytics.nn.tasks.BaseModel.init_criterion

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/nn/tasks.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: torch.nn.Module

Base class for all YOLO models in the Ultralytics family.

This class provides common functionality for YOLO models including forward pass handling, model fusion, information display, and weight loading capabilities.

Apply a function to all tensors in the model that are not parameters or registered buffers.

Perform augmentations on input image x and return augmented inference.

Perform a forward pass through the network.

Profile the computation time and FLOPs of a single layer of the model on a given input.

Perform forward pass of the model for either training or inference.

If x is a dict, calculates and returns the loss for training. Otherwise, returns predictions for inference.

Fuse the Conv2d() and BatchNorm2d() layers of the model into a single layer for improved computation

Print model information.

Initialize the loss criterion for the BaseModel.

Check if the model has less than a certain threshold of BatchNorm layers.

Load weights into the model.

Perform a forward pass through the network.

YOLO detection model.

This class implements the YOLO detection architecture, handling model initialization, forward pass, augmented inference, and loss computation for object detection tasks.

Clip YOLO augmented inference tails.

De-scale predictions following augmented inference (inverse operation).

Perform augmentations on input image x and return augmented inference and train outputs.

Initialize the loss criterion for the DetectionModel.

Bases: DetectionModel

YOLO Oriented Bounding Box (OBB) model.

This class extends DetectionModel to handle oriented bounding box detection tasks, providing specialized loss computation for rotated object detection.

Initialize the loss criterion for the model.

Bases: DetectionModel

YOLO segmentation model.

This class extends DetectionModel to handle instance segmentation tasks, providing specialized loss computation for pixel-level object detection and segmentation.

Initialize the loss criterion for the SegmentationModel.

Bases: DetectionModel

This class extends DetectionModel to handle human pose estimation tasks, providing specialized loss computation for keypoint detection and pose estimation.

Initialize the loss criterion for the PoseModel.

YOLO classification model.

This class implements the YOLO classification architecture for image classification tasks, providing model initialization, configuration, and output reshaping capabilities.

Set Ultralytics YOLO model configurations and define the model architecture.

Initialize the loss criterion for the ClassificationModel.

Update a TorchVision classification model to class count 'n' if required.

Bases: DetectionModel

RTDETR (Real-time DEtection and Tracking using Transformers) Detection Model class.

This class is responsible for constructing the RTDETR architecture, defining loss functions, and facilitating both the training and inference processes. RTDETR is an object detection and tracking model that extends from the DetectionModel base class.

Apply a function to all tensors in the model that are not parameters or registered buffers.

Initialize the loss criterion for the RTDETRDetectionModel.

Compute the loss for the given batch of data.

Perform a forward pass through the model.

Bases: DetectionModel

This class implements the YOLOv8 World model for open-vocabulary object detection, supporting text-based class specification and CLIP model integration for zero-shot detection capabilities.

Get text positional embeddings for offline inference without CLIP model.

Perform a forward pass through the model.

Set classes in advance so that model could do offline-inference without clip model.

Bases: DetectionModel

YOLOE detection model.

This class implements the YOLOE architecture for efficient object detection with text and visual prompts, supporting both prompt-based and prompt-free inference modes.

Get class positional embeddings.

Get text positional embeddings for offline inference without CLIP model.

Get visual embeddings.

Get fused vocabulary layer from the model.

Perform a forward pass through the model.

Set classes in advance so that model could do offline-inference without clip model.

Set vocabulary for the prompt-free model.

Bases: YOLOEModel, SegmentationModel

YOLOE segmentation model.

This class extends YOLOEModel to handle instance segmentation tasks with text and visual prompts, providing specialized loss computation for pixel-level object detection and segmentation.

Bases: torch.nn.ModuleList

This class allows combining multiple YOLO models into an ensemble for improved performance through model averaging or other ensemble techniques.

Generate the YOLO network's final layer.

A placeholder class to replace unknown classes during unpickling.

Run SafeClass instance, ignoring all arguments.

Bases: pickle.Unpickler

Custom Unpickler that replaces unknown classes with SafeClass.

Attempt to find a class, returning SafeClass if not among safe modules.

Context manager for temporarily adding or modifying modules in Python's module cache (sys.modules).

This function can be used to change the module paths during runtime. It's useful when refactoring code, where you've moved a module from one location to another, but you still want to support the old import paths for backwards compatibility.

The changes are only in effect inside the context manager and are undone once the context manager exits. Be aware that directly manipulating sys.modules can lead to unpredictable results, especially in larger applications or libraries. Use this function with caution.

Attempt to load a PyTorch model with the torch.load() function. If a ModuleNotFoundError is raised, it catches

the error, logs a warning message, and attempts to install the missing module via the check_requirements() function. After installation, the function again attempts to load the model using torch.load().

Load a single model weights.

Parse a YOLO model.yaml dictionary into a PyTorch model.

Load a YOLOv8 model from a YAML file.

Extract the size character n, s, m, l, or x of the model's scale from the model path.

Guess the task of a PyTorch model from its architecture or configuration.

**Examples:**

Example 1 (unknown):
```unknown
BaseModel()
```

Example 2 (unknown):
```unknown
Create a BaseModel instance
>>> model = BaseModel()
>>> model.info()  # Display model information
```

Example 3 (php):
```php
class BaseModel(torch.nn.Module):
```

Example 4 (python):
```python
def _apply(self, fn)
```

---

## Reference for ultralytics/solutions/region_counter.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/region_counter/

**Contents:**
- Reference for ultralytics/solutions/region_counter.py
- class ultralytics.solutions.region_counter.RegionCounter
  - method ultralytics.solutions.region_counter.RegionCounter.add_region
  - method ultralytics.solutions.region_counter.RegionCounter.initialize_regions
  - method ultralytics.solutions.region_counter.RegionCounter.process

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/region_counter.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class for real-time counting of objects within user-defined regions in a video stream.

This class inherits from BaseSolution and provides functionality to define polygonal regions in a video frame, track objects, and count those objects that pass through each defined region. Useful for applications requiring counting in specified areas, such as monitoring zones or segmented sections.

Add a new region to the counting list based on the provided template with specific attributes.

Initialize regions from self.region only once.

Process the input frame to detect and count objects within each defined region.

**Examples:**

Example 1 (rust):
```rust
RegionCounter(self, **kwargs: Any) -> None
```

Example 2 (json):
```json
Initialize a RegionCounter and add a counting region
>>> counter = RegionCounter()
>>> counter.add_region("Zone1", [(100, 100), (200, 100), (200, 200), (100, 200)], (255, 0, 0), (255, 255, 255))
>>> results = counter.process(frame)
>>> print(f"Total tracks: {results.total_tracks}")
```

Example 3 (python):
```python
class RegionCounter(BaseSolution):
    """A class for real-time counting of objects within user-defined regions in a video stream.

    This class inherits from `BaseSolution` and provides functionality to define polygonal regions in a video frame,
    track objects, and count those objects that pass through each defined region. Useful for applications requiring
    counting in specified areas, such as monitoring zones or segmented sections.

    Attributes:
        region_template (dict): Template for creating new counting regions with default attributes including name,
            polygon coordinates, and display colors.
        counting_regions (list): List storing all defined regions, where each entry is based on `region_template` and
            includes specific region settings like name, coordinates, and color.
        region_counts (dict): Dictionary storing the count of objects for each named region.

    Methods:
        add_region: Add a new counting region with specified attributes.
        process: Process video frames to count objects in each region.
        initialize_regions: Initialize zones to count the objects in each one. Zones could be multiple as well.

    Examples:
        Initialize a RegionCounter and add a counting region
        >>> counter = RegionCounter()
        >>> counter.add_region("Zone1", [(100, 100), (200, 100), (200, 200), (100, 200)], (255, 0, 0), (255, 255, 255))
        >>> results = counter.process(frame)
        >>> print(f"Total tracks: {results.total_tracks}")
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the RegionCounter for real-time object counting in user-defined regions."""
        super().__init__(**kwargs)
        self.region_template = {
            "name": "Default Region",
            "polygon": None,
            "counts": 0,
            "region_color": (255, 255, 255),
            "text_color": (0, 0, 0),
        }
        self.region_counts = {}
        self.counting_regions = []
        self.initialize_regions()
```

Example 4 (python):
```python
def add_region(
    self,
    name: str,
    polygon_points: list[tuple],
    region_color: tuple[int, int, int],
    text_color: tuple[int, int, int],
) -> dict[str, Any]
```

---

## Reference for hub_sdk/modules/users.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/modules/users/

**Contents:**
- Reference for hub_sdk/modules/users.py
- class hub_sdk.modules.users.Users
  - method hub_sdk.modules.users.Users.create_user
  - method hub_sdk.modules.users.Users.delete
  - method hub_sdk.modules.users.Users.get_data
  - method hub_sdk.modules.users.Users.update

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/modules/users.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class representing a client for interacting with Users through CRUD operations.

This class extends the CRUDClient class and provides specific methods for working with Users.

The 'id' attribute is set during initialization and can be used to uniquely identify a user. The 'data' attribute is used to store user data fetched from the API.

Create a new user with the provided data and set the user ID for the current instance.

Delete the user resource represented by this instance.

The 'hard' parameter determines whether to perform a soft delete (default) or a hard delete. In a soft delete, the model might be marked as deleted but retained in the system. In a hard delete, the model is permanently removed from the system.

Retrieve data for the current user instance.

If a valid user ID has been set, it sends a request to fetch the user data and stores it in the instance. If no user ID has been set, it logs an error message.

Update the user resource represented by this instance.

**Examples:**

Example 1 (rust):
```rust
Users(self, user_id: str | None = None, headers: dict[str, Any] | None = None) -> None
```

Example 2 (python):
```python
class Users(CRUDClient):
    """A class representing a client for interacting with Users through CRUD operations.

    This class extends the CRUDClient class and provides specific methods for working with Users.

    Attributes:
        id (str | None): The unique identifier of the user, if available.
        data (Dict): A dictionary to store user data.

    Methods:
        get_data: Retrieves data for the current user instance.
        create_user: Creates a new user with the provided data.
        delete: Deletes the user resource represented by this instance.
        update: Updates the user resource represented by this instance.

    Notes:
        The 'id' attribute is set during initialization and can be used to uniquely identify a user.
        The 'data' attribute is used to store user data fetched from the API.
    """

    def __init__(self, user_id: str | None = None, headers: dict[str, Any] | None = None) -> None:
        """Initialize a Users object for interacting with user data via CRUD operations.

        Args:
            user_id (str, optional): The unique identifier of the user.
            headers (Dict[str, Any], optional): A dictionary of HTTP headers to be included in API requests.
        """
        super().__init__("users", "user", headers)
        self.id = user_id
        self.data = {}
        if user_id:
            self.get_data()
```

Example 3 (python):
```python
def create_user(self, user_data: dict) -> None
```

Example 4 (python):
```python
def create_user(self, user_data: dict) -> None:
    """Create a new user with the provided data and set the user ID for the current instance.

    Args:
        user_data (Dict): A dictionary containing the data for creating the user.
    """
    resp = super().create(user_data).json()
    self.id = resp.get("data", {}).get("id")
    self.get_data()
```

---

## Reference for ultralytics/nn/modules/activation.py

**URL:** https://docs.ultralytics.com/zh/reference/nn/modules/activation/

**Contents:**
- Reference for ultralytics/nn/modules/activation.py
- class ultralytics.nn.modules.activation.AGLU
  - method ultralytics.nn.modules.activation.AGLU.forward

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/nn/modules/activation.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Unified activation function module from AGLU.

This class implements a parameterized activation function with learnable parameters lambda and kappa, based on the AGLU (Adaptive Gated Linear Unit) approach.

Apply the Adaptive Gated Linear Unit (AGLU) activation function.

This forward method implements the AGLU activation function with learnable parameters lambda and kappa. The function applies a transformation that adaptively combines linear and non-linear components.

**Examples:**

Example 1 (rust):
```rust
AGLU(self, device = None, dtype = None) -> None
```

Example 2 (yaml):
```yaml
>>> import torch
    >>> m = AGLU()
    >>> input = torch.randn(2)
    >>> output = m(input)
    >>> print(output.shape)
    torch.Size([2])

References:
    https://github.com/kostas1515/AGLU
```

Example 3 (python):
```python
class AGLU(nn.Module):
    """Unified activation function module from AGLU.

    This class implements a parameterized activation function with learnable parameters lambda and kappa, based on the
    AGLU (Adaptive Gated Linear Unit) approach.

    Attributes:
        act (nn.Softplus): Softplus activation function with negative beta.
        lambd (nn.Parameter): Learnable lambda parameter initialized with uniform distribution.
        kappa (nn.Parameter): Learnable kappa parameter initialized with uniform distribution.

    Methods:
        forward: Compute the forward pass of the Unified activation function.

    Examples:
        >>> import torch
        >>> m = AGLU()
        >>> input = torch.randn(2)
        >>> output = m(input)
        >>> print(output.shape)
        torch.Size([2])

    References:
        https://github.com/kostas1515/AGLU
    """

    def __init__(self, device=None, dtype=None) -> None:
        """Initialize the Unified activation function with learnable parameters."""
        super().__init__()
        self.act = nn.Softplus(beta=-1.0)
        self.lambd = nn.Parameter(nn.init.uniform_(torch.empty(1, device=device, dtype=dtype)))  # lambda parameter
        self.kappa = nn.Parameter(nn.init.uniform_(torch.empty(1, device=device, dtype=dtype)))  # kappa parameter
```

Example 4 (python):
```python
def forward(self, x: torch.Tensor) -> torch.Tensor
```

---

## Reference for ultralytics/nn/modules/conv.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/nn/modules/conv/

**Contents:**
- Reference for ultralytics/nn/modules/conv.py
- class ultralytics.nn.modules.conv.Conv
  - method ultralytics.nn.modules.conv.Conv.forward
  - method ultralytics.nn.modules.conv.Conv.forward_fuse
- class ultralytics.nn.modules.conv.Conv2
  - method ultralytics.nn.modules.conv.Conv2.forward
  - method ultralytics.nn.modules.conv.Conv2.forward_fuse
  - method ultralytics.nn.modules.conv.Conv2.fuse_convs
- class ultralytics.nn.modules.conv.LightConv
  - method ultralytics.nn.modules.conv.LightConv.forward

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/nn/modules/conv.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Standard convolution module with batch normalization and activation.

Apply convolution, batch normalization and activation to input tensor.

Apply convolution and activation without batch normalization.

Simplified RepConv module with Conv fusing.

Apply convolution, batch normalization and activation to input tensor.

Apply fused convolution, batch normalization and activation to input tensor.

Fuse parallel convolutions.

Light convolution module with 1x1 and depthwise convolutions.

This implementation is based on the PaddleDetection HGNetV2 backbone.

Apply 2 convolutions to input tensor.

Depth-wise convolution module.

Bases: nn.ConvTranspose2d

Depth-wise transpose convolution module.

Convolution transpose module with optional batch normalization and activation.

Apply transposed convolution, batch normalization and activation to input.

Apply activation and convolution transpose operation to input.

Focus module for concentrating feature information.

Slices input tensor into 4 parts and concatenates them in the channel dimension.

Apply Focus operation and convolution to input tensor.

Input shape is (B, C, W, H) and output shape is (B, 4C, W/2, H/2).

Ghost Convolution module.

Generates more features with fewer parameters by using cheap operations.

Apply Ghost Convolution to input tensor.

RepConv module with training and deploy modes.

This module is used in RT-DETR and can fuse convolutions during inference for efficiency.

Fuse batch normalization with convolution weights.

Pad a 1x1 kernel to 3x3 size.

Forward pass for training mode.

Forward pass for deploy mode.

Fuse convolutions for inference by creating a single equivalent convolution.

Calculate equivalent kernel and bias by fusing convolutions.

Channel-attention module for feature recalibration.

Applies attention weights to channels based on global average pooling.

Apply channel attention to input tensor.

Spatial-attention module for feature recalibration.

Applies attention weights to spatial dimensions based on channel statistics.

Apply spatial attention to input tensor.

Convolutional Block Attention Module.

Combines channel and spatial attention mechanisms for comprehensive feature refinement.

Apply channel and spatial attention sequentially to input tensor.

Concatenate a list of tensors along specified dimension.

Concatenate input tensors along specified dimension.

Returns a particular index of the input.

Select and return a particular index from input.

Pad to 'same' shape outputs.

**Examples:**

Example 1 (rust):
```rust
Conv(self, c1, c2, k = 1, s = 1, p = None, g = 1, d = 1, act = True)
```

Example 2 (python):
```python
class Conv(nn.Module):
    """Standard convolution module with batch normalization and activation.

    Attributes:
        conv (nn.Conv2d): Convolutional layer.
        bn (nn.BatchNorm2d): Batch normalization layer.
        act (nn.Module): Activation function layer.
        default_act (nn.Module): Default activation function (SiLU).
    """

    default_act = nn.SiLU()  # default activation

    def __init__(self, c1, c2, k=1, s=1, p=None, g=1, d=1, act=True):
        """Initialize Conv layer with given parameters.

        Args:
            c1 (int): Number of input channels.
            c2 (int): Number of output channels.
            k (int): Kernel size.
            s (int): Stride.
            p (int, optional): Padding.
            g (int): Groups.
            d (int): Dilation.
            act (bool | nn.Module): Activation function.
        """
        super().__init__()
        self.conv = nn.Conv2d(c1, c2, k, s, autopad(k, p, d), groups=g, dilation=d, bias=False)
        self.bn = nn.BatchNorm2d(c2)
        self.act = self.default_act if act is True else act if isinstance(act, nn.Module) else nn.Identity()
```

Example 3 (python):
```python
def forward(self, x)
```

Example 4 (python):
```python
def forward(self, x):
    """Apply convolution, batch normalization and activation to input tensor.

    Args:
        x (torch.Tensor): Input tensor.

    Returns:
        (torch.Tensor): Output tensor.
    """
    return self.act(self.bn(self.conv(x)))
```

---

## Ultralytics 个人贡献者许可协议

**URL:** https://docs.ultralytics.com/zh/help/CLA/

**Contents:**
- Ultralytics 个人贡献者许可协议
- 1. 定义
  - 1.1 “您”或“您的”
  - 1.2 “贡献”
  - 1.3 “版权”
  - 1.4 “提交”或“提交物”或“已提交”
  - 1.5 “项目”
- 2. 权利授予
  - 2.1 版权许可
  - 2.2 专利许可

感谢您对参与 Ultralytics Inc.（“Ultralytics”、“我们”）管理之软件项目的兴趣。本贡献者许可协议（“协议”）规定了贡献者（“您”）授予我们的权利，以及管辖第 1 节中定义的任何贡献的条款。此许可既是为了保护您作为贡献者，也是为了保护 Ultralytics；它不会改变您出于任何其他目的使用您自己贡献的权利。

通过接受并同意这些条款和条件，您接受并同意以下条款和条件，适用于您过去、现在和将来提交给 Ultralytics 的贡献。除本协议授予 Ultralytics 和 Ultralytics 分发的软件接收者的许可外，您保留对您的贡献的所有权利、所有权和利益。

如果您对本协议有任何疑问，请联系 hello@ultralytics.com。

指将贡献提交给 Ultralytics 的个人，或经版权所有者授权与 Ultralytics 签订本协议的法律实体。对于法律实体，进行贡献的实体以及控制该实体、受该实体控制或与该实体处于共同控制下的所有其他实体均被视为单个贡献者。就本定义而言，“控制”是指 (i) 通过合同或其他方式直接或间接导致该实体定向或管理的能力，或 (ii) 拥有百分之五十 (50%) 或以上已发行股份的所有权，或 (iii) 该实体的实益所有权。

指您有意以任何形式和任何方式提交给 Ultralytics 的任何原创作品，包括但不限于源代码、目标代码、错误修复、配置更改、工具、规范、文档、数据、材料、反馈、信息或任何其他作品，以包含在 Ultralytics 管理的任何项目中或作为其文档的一部分（“作品”）。这包括为贡献于项目和改进作品而提交的对现有作品的任何修改或添加。

指保护您拥有或控制的作品的所有权利，包括版权、精神权利和邻接权（如适用），在其存在的整个期限内（包括您的任何延期）。

或任何衍生品应指发送给 Ultralytics 或其代表的任何形式的电子、口头或书面通信，包括但不限于电子邮件列表、源代码控制系统和问题跟踪系统上的通信，这些通信由 Ultralytics 管理或代表 Ultralytics 管理，目的是讨论和改进项目，但不包括您以书面形式明确标记或以其他方式指定为“非贡献”的通信。

指 Ultralytics 拥有、管理或维护的任何软件项目，包括但不限于 Ultralytics 提供的可以提交贡献的开源项目。

在相关法律允许的最大范围内，并受本协议条款和条件的约束，您特此授予 Ultralytics 和 Ultralytics 分发的软件的接收者永久的、全球性的、非独占的、免版税的、不可撤销的版权许可，以复制、准备衍生作品、公开展示、公开表演、再许可和分发您的贡献以及此类衍生作品。

在相关法律允许的最大范围内，并受本协议条款和条件的约束，您特此授予 Ultralytics 和 Ultralytics 分发的软件的接收者永久的、全球性的、非独占的、免版税的、不可撤销的（除非本节另有规定）专利许可，以制造、委托制造、使用、提议销售、销售、进口和以其他方式转让作品，其中此类许可仅适用于您可许可的、因您的贡献单独或您的贡献与提交该贡献的作品相结合而必然侵犯的那些专利权利要求。如果任何实体对您或任何其他实体（包括诉讼中的交叉索赔或反诉）提起专利诉讼，声称您的贡献或您已贡献的作品构成直接或间接的专利侵权，则根据本协议授予该实体的关于该贡献或作品的任何专利许可应自提起该诉讼之日起终止。

基于第 2.1 和 2.2 节中授予的权利，如果我们将您的贡献包含在材料中，我们可以根据任何许可（包括 copyleft、宽松、商业或专有许可）许可该贡献。

在法律允许的最大范围内，您特此放弃并同意不主张您在您的贡献中或与之相关的所有“精神权利”，以使 Ultralytics、其受让人及其各自的直接和间接再许可人受益。

（b）您拥有涵盖根据第 2 节授予权利所需的贡献的版权和专利权。

（c）第 2 节项下的权利授予不违反您已向第三方（包括您的雇主）做出的任何权利授予。如果您的贡献是在您过去或现在的雇主处工作期间创作的，您声明该雇主已授权您代表该雇主做出贡献，或者该雇主已放弃其对您的贡献的所有权利、所有权或利益。

（d）如果您不拥有提交的整个作品的版权，您已遵循 Ultralytics 提供的说明。

（e）如果您希望提交不是您原创的作品，您可以将其与任何贡献分开提交给 Ultralytics，并注明其来源的完整详细信息以及您个人知道的任何许可或其他限制（包括但不限于相关的专利、商标和许可协议），并以醒目的方式将该作品标记为“代表第三方提交：[在此处命名]”。

（f）您同意将您了解的任何事实或情况通知 Ultralytics，这些事实或情况会使这些陈述在任何方面都不准确。

除第 3 节中的明示保证外，贡献按“原样”提供。更具体地说，您在此明确声明不对我们做出任何明示或暗示的保证，包括但不限于任何关于适销性、适用于特定目的和不侵权的暗示保证。如果任何此类保证无法免除，则此类保证的期限应限于法律允许的最短期限。

本协议受美国纽约州法律管辖并按其解释，但不包括其法律冲突条款。双方同意，与本协议有关的所有事宜，均由位于纽约州纽约市的法院管辖并审理。您放弃所有关于缺乏个人管辖权和不方便法院的抗辩。

本协议构成您与Ultralytics之间关于您的贡献的完整协议，并取代所有其他协议或谅解。

Ultralytics可以转让本协议及其项下的所有权利、义务和许可，无需事先征得您的同意。

任何一方在一种情况下未能要求另一方履行本协议的任何条款，不应影响该方在未来任何时候要求履行该条款的权利。在一种情况下放弃履行某一条款，不应被视为放弃在未来履行该条款，也不应被视为完全放弃该条款。

如果本协议的任何条款被认定为无效且不可执行，则该条款应在可能的范围内被替换为最接近原始条款含义且可执行的条款。即使本协议的基本目的未能实现或任何有限的补救措施未能奏效，本协议中规定的条款和条件仍应在法律允许的最大范围内适用。

您承认Ultralytics没有义务将您的贡献用于或纳入任何作品中。是否将您的贡献用于或纳入任何作品中的决定将由Ultralytics或其授权代表自行决定。

本协议的生效日期应为您签署本协议之日或您首次向Ultralytics提交贡献之日，以较早者为准。

Ultralytics CLA 定义了您向 Ultralytics 软件项目贡献内容的条款。它概述了与您的贡献相关的权利和义务，包括授予版权和专利许可，以及处理第三方内容。

同意版权许可允许 Ultralytics 及其用户使用、修改、分发您的贡献并基于您的贡献创建衍生作品。这确保了您的贡献可以集成到 Ultralytics 项目 中并与社区共享，从而促进协作和软件开发。

该专利许可授予 Ultralytics 使用、制造和销售您的专利所涵盖的贡献的权利。这对于产品开发和商业化至关重要。作为回报，您获得专利的创新将获得更广泛的使用和认可，从而促进社区内的创新。该专利许可类似于 AGPL-3.0 等其他开源许可中的条款。

如果您的贡献包含第三方内容，您必须明确标明，并提供关于其来源和任何适用许可或限制的全面详细信息。这确保了 Ultralytics 项目中适当的归属和法律合规性，从而保持透明度并尊重原始内容创建者的权利。

Ultralytics 没有义务使用或将您的贡献纳入任何项目中。使用您的贡献的决定完全由 Ultralytics 自行决定，这意味着虽然您的贡献很有价值，但它们可能并不总是与项目的当前需求或方向相符。

如果您对贡献者许可协议有任何其他问题或需要澄清，请通过 hello@ultralytics.com 与我们联系。有关为 Ultralytics 项目做出贡献的更多信息，请参阅我们的贡献指南。

---

## Reference for ultralytics/__init__.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/__init__/

**Contents:**
- Reference for ultralytics/__init__.py
- function ultralytics.__getattr__
- function ultralytics.__dir__

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/__init__.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Lazy-import model classes on first access.

Extend dir() to include lazily available model names for IDE autocompletion.

**Examples:**

Example 1 (python):
```python
def __getattr__(name: str)
```

Example 2 (python):
```python
def __getattr__(name: str):
    """Lazy-import model classes on first access."""
    if name in MODELS:
        return getattr(importlib.import_module("ultralytics.models"), name)
    raise AttributeError(f"module {__name__} has no attribute {name}")
```

Example 3 (python):
```python
def __dir__()
```

Example 4 (python):
```python
def __dir__():
    """Extend dir() to include lazily available model names for IDE autocompletion."""
    return sorted(set(globals()) | set(MODELS))
```

---

## Reference for ultralytics/engine/results.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/engine/results/

**Contents:**
- Reference for ultralytics/engine/results.py
- class ultralytics.engine.results.BaseTensor
  - property ultralytics.engine.results.BaseTensor.shape
  - method ultralytics.engine.results.BaseTensor.__getitem__
  - method ultralytics.engine.results.BaseTensor.__len__
  - method ultralytics.engine.results.BaseTensor.cpu
  - method ultralytics.engine.results.BaseTensor.cuda
  - method ultralytics.engine.results.BaseTensor.numpy
  - method ultralytics.engine.results.BaseTensor.to
- class ultralytics.engine.results.Results

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/engine/results.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Base tensor class with additional methods for easy manipulation and device handling.

This class provides a foundation for tensor-like objects with device management capabilities, supporting both PyTorch tensors and NumPy arrays. It includes methods for moving data between devices and converting between tensor types.

Return the shape of the underlying data tensor.

Return a new BaseTensor instance containing the specified indexed elements of the data tensor.

Return the length of the underlying data tensor.

Return a copy of the tensor stored in CPU memory.

Move the tensor to GPU memory.

Return a copy of this object with its data converted to a NumPy array.

Return a copy of the tensor with the specified device and dtype.

Bases: SimpleClass, DataExportMixin

A class for storing and manipulating inference results.

This class provides comprehensive functionality for handling inference results from various Ultralytics models, including detection, segmentation, classification, and pose estimation. It supports visualization, data export, and various coordinate transformations.

For the default pose model, keypoint indices for human body pose estimation are: 0: Nose, 1: Left Eye, 2: Right Eye, 3: Left Ear, 4: Right Ear 5: Left Shoulder, 6: Right Shoulder, 7: Left Elbow, 8: Right Elbow 9: Left Wrist, 10: Right Wrist, 11: Left Hip, 12: Right Hip 13: Left Knee, 14: Right Knee, 15: Left Ankle, 16: Right Ankle

Return a Results object for a specific index of inference results.

Return the number of detections in the Results object.

Apply a function to all non-empty attributes and return a new Results object with modified attributes.

This method is internally called by methods like .to(), .cuda(), .cpu(), etc.

Return a copy of the Results object with all its tensors moved to CPU memory.

This method creates a new Results object with all tensor attributes (boxes, masks, probs, keypoints, obb) transferred to CPU memory. It's useful for moving data from GPU to CPU for further processing or saving.

Move all tensors in the Results object to GPU memory.

Create a new Results object with the same image, path, names, and speed attributes.

Convert all tensors in the Results object to numpy arrays.

This method creates a new Results object, leaving the original unchanged. It's useful for interoperability with numpy-based libraries or when CPU-based operations are required.

Plot detection results on an input BGR image.

Save annotated inference results image to file.

This method plots the detection results on the original image and saves the annotated image to a file. It utilizes the plot method to generate the annotated image and then saves it to the specified filename.

Save cropped detection images to specified directory.

This method saves cropped images of detected objects to a specified directory. Each crop is saved in a subdirectory named after the object's class, with the filename based on the input file_name.

Save detection results to a text file.

Display the image with annotated inference results.

This method plots the detection results on the original image and displays it. It's a convenient way to visualize the model's predictions directly.

Convert inference results to a summarized dictionary with optional normalization for box coordinates.

This method creates a list of detection dictionaries, each containing information about a single detection or classification result. For classification tasks, it returns the top class and its confidence. For detection tasks, it includes class information, bounding box coordinates, and optionally mask segments and keypoints.

Move all tensors in the Results object to the specified device and dtype.

Update the Results object with new detection data.

This method allows updating the boxes, masks, probabilities, and oriented bounding boxes (OBB) of the Results object. It ensures that boxes are clipped to the original image shape.

Return a log string for each task in the results, detailing detection and classification outcomes.

This method generates a human-readable string summarizing the detection and classification results. It includes the number of detections for each class and the top probabilities for classification tasks.

A class for managing and manipulating detection boxes.

This class provides comprehensive functionality for handling detection boxes, including their coordinates, confidence scores, class labels, and optional tracking IDs. It supports various box formats and offers methods for easy manipulation and conversion between different coordinate systems.

This class manages detection boxes, providing easy access and manipulation of box coordinates, confidence scores, class identifiers, and optional tracking IDs. It supports multiple formats for box coordinates, including both absolute and normalized forms.

Return bounding boxes in [x1, y1, x2, y2] format.

Return the confidence scores for each detection box.

Return the class ID tensor representing category predictions for each bounding box.

Return the tracking IDs for each detection box if available.

Convert bounding boxes from [x1, y1, x2, y2] format to [x, y, width, height] format.

Return normalized bounding box coordinates relative to the original image size.

This property calculates and returns the bounding box coordinates in [x1, y1, x2, y2] format, normalized to the range [0, 1] based on the original image dimensions.

Return normalized bounding boxes in [x, y, width, height] format.

This property calculates and returns the normalized bounding box coordinates in the format [x_center, y_center, width, height], where all values are relative to the original image dimensions.

A class for storing and manipulating detection masks.

This class extends BaseTensor and provides functionality for handling segmentation masks, including methods for converting between pixel and normalized coordinates.

Return normalized xy-coordinates of the segmentation masks.

This property calculates and caches the normalized xy-coordinates of the segmentation masks. The coordinates are normalized relative to the original image shape.

Return the [x, y] pixel coordinates for each segment in the mask tensor.

This property calculates and returns a list of pixel coordinates for each segmentation mask in the Masks object. The coordinates are scaled to match the original image dimensions.

A class for storing and manipulating detection keypoints.

This class encapsulates functionality for handling keypoint data, including coordinate manipulation, normalization, and confidence values. It supports keypoint detection results with optional visibility information.

This method processes the input keypoints tensor, handling both 2D and 3D formats. For 3D tensors (x, y, confidence), it masks out low-confidence keypoints by setting their coordinates to zero.

Return x, y coordinates of keypoints.

Return normalized coordinates (x, y) of keypoints relative to the original image size.

Return confidence values for each keypoint.

A class for storing and manipulating classification probabilities.

This class extends BaseTensor and provides methods for accessing and manipulating classification probabilities, including top-1 and top-5 predictions.

This class stores and manages classification probabilities, providing easy access to top predictions and their confidences.

Return the index of the class with the highest probability.

Return the indices of the top 5 class probabilities.

Return the confidence score of the highest probability class.

This property retrieves the confidence score (probability) of the class with the highest predicted probability from the classification results.

Return confidence scores for the top 5 classification predictions.

This property retrieves the confidence scores corresponding to the top 5 class probabilities predicted by the model. It provides a quick way to access the most likely class predictions along with their associated confidence levels.

A class for storing and manipulating Oriented Bounding Boxes (OBB).

This class provides functionality to handle oriented bounding boxes, including conversion between different formats, normalization, and access to various properties of the boxes. It supports both tracking and non-tracking scenarios.

This class stores and manipulates Oriented Bounding Boxes (OBB) for object detection tasks. It provides various properties and methods to access and transform the OBB data.

Return boxes in [x_center, y_center, width, height, rotation] format.

Return the confidence scores for Oriented Bounding Boxes (OBBs).

This property retrieves the confidence values associated with each OBB detection. The confidence score represents the model's certainty in the detection.

Return the class values of the oriented bounding boxes.

Return the tracking IDs of the oriented bounding boxes (if available).

Convert OBB format to 8-point (xyxyxyxy) coordinate format for rotated bounding boxes.

Convert rotated bounding boxes to normalized xyxyxyxy format.

Convert oriented bounding boxes (OBB) to axis-aligned bounding boxes in xyxy format.

This property calculates the minimal enclosing rectangle for each oriented bounding box and returns it in xyxy format (x1, y1, x2, y2). This is useful for operations that require axis-aligned bounding boxes, such as IoU calculation with non-rotated boxes.

**Examples:**

Example 1 (rust):
```rust
BaseTensor(self, data: torch.Tensor | np.ndarray, orig_shape: tuple[int, int]) -> None
```

Example 2 (python):
```python
>>> import torch
>>> data = torch.tensor([[1, 2, 3], [4, 5, 6]])
>>> orig_shape = (720, 1280)
>>> base_tensor = BaseTensor(data, orig_shape)
>>> cpu_tensor = base_tensor.cpu()
>>> numpy_array = base_tensor.numpy()
>>> gpu_tensor = base_tensor.cuda()
```

Example 3 (python):
```python
class BaseTensor(SimpleClass):
    """Base tensor class with additional methods for easy manipulation and device handling.

    This class provides a foundation for tensor-like objects with device management capabilities, supporting both
    PyTorch tensors and NumPy arrays. It includes methods for moving data between devices and converting between tensor
    types.

    Attributes:
        data (torch.Tensor | np.ndarray): Prediction data such as bounding boxes, masks, or keypoints.
        orig_shape (tuple[int, int]): Original shape of the image, typically in the format (height, width).

    Methods:
        cpu: Return a copy of the tensor stored in CPU memory.
        numpy: Return a copy of the tensor as a numpy array.
        cuda: Move the tensor to GPU memory, returning a new instance if necessary.
        to: Return a copy of the tensor with the specified device and dtype.

    Examples:
        >>> import torch
        >>> data = torch.tensor([[1, 2, 3], [4, 5, 6]])
        >>> orig_shape = (720, 1280)
        >>> base_tensor = BaseTensor(data, orig_shape)
        >>> cpu_tensor = base_tensor.cpu()
        >>> numpy_array = base_tensor.numpy()
        >>> gpu_tensor = base_tensor.cuda()
    """

    def __init__(self, data: torch.Tensor | np.ndarray, orig_shape: tuple[int, int]) -> None:
        """Initialize BaseTensor with prediction data and the original shape of the image.

        Args:
            data (torch.Tensor | np.ndarray): Prediction data such as bounding boxes, masks, or keypoints.
            orig_shape (tuple[int, int]): Original shape of the image in (height, width) format.
        """
        assert isinstance(data, (torch.Tensor, np.ndarray)), "data must be torch.Tensor or np.ndarray"
        self.data = data
        self.orig_shape = orig_shape
```

Example 4 (python):
```python
def shape(self) -> tuple[int, ...]
```

---

## Reference for ultralytics/hub/auth.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/hub/auth/

**Contents:**
- Reference for ultralytics/hub/auth.py
- class ultralytics.hub.auth.Auth
  - method ultralytics.hub.auth.Auth.auth_with_cookies
  - method ultralytics.hub.auth.Auth.authenticate
  - method ultralytics.hub.auth.Auth.get_auth_header
  - method ultralytics.hub.auth.Auth.request_api_key

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/hub/auth.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Manages authentication processes including API key handling, cookie-based authentication, and header generation.

The class supports different methods of authentication: 1. Directly using an API key. 2. Authenticating using browser cookies (specifically in Google Colab). 3. Prompting the user to enter an API key.

Handles API key validation, Google Colab authentication, and new key requests. Updates SETTINGS upon successful authentication.

Attempt to fetch authentication via cookies and set id_token.

User must be logged in to HUB and running in a supported browser.

Attempt to authenticate with the server using either id_token or API key.

Get the authentication header for making API requests.

Prompt the user to input their API key.

**Examples:**

Example 1 (typescript):
```typescript
Auth(self, api_key: str = "", verbose: bool = False)
```

Example 2 (unknown):
```unknown
Initialize Auth with an API key
>>> auth = Auth(api_key="your_api_key_here")

Initialize Auth without API key (will prompt for input)
>>> auth = Auth()
```

Example 3 (python):
```python
class Auth:
    """Manages authentication processes including API key handling, cookie-based authentication, and header generation.

    The class supports different methods of authentication:
    1. Directly using an API key.
    2. Authenticating using browser cookies (specifically in Google Colab).
    3. Prompting the user to enter an API key.

    Attributes:
        id_token (str | bool): Token used for identity verification, initialized as False.
        api_key (str | bool): API key for authentication, initialized as False.
        model_key (bool): Placeholder for model key, initialized as False.

    Methods:
        authenticate: Attempt to authenticate with the server using either id_token or API key.
        auth_with_cookies: Attempt to fetch authentication via cookies and set id_token.
        get_auth_header: Get the authentication header for making API requests.
        request_api_key: Prompt the user to input their API key.

    Examples:
        Initialize Auth with an API key
        >>> auth = Auth(api_key="your_api_key_here")

        Initialize Auth without API key (will prompt for input)
        >>> auth = Auth()
    """

    id_token = api_key = model_key = False

    def __init__(self, api_key: str = "", verbose: bool = False):
        """Initialize Auth class and authenticate user.

        Handles API key validation, Google Colab authentication, and new key requests. Updates SETTINGS upon successful
        authentication.

        Args:
            api_key (str): API key or combined key_id format.
            verbose (bool): Enable verbose logging.
        """
        # Split the input API key in case it contains a combined key_model and keep only the API key part
        api_key = api_key.split("_", 1)[0]

        # Set API key attribute as value passed or SETTINGS API key if none passed
        self.api_key = api_key or SETTINGS.get("api_key", "")

        # If an API key is provided
        if self.api_key:
            # If the provided API key matches the API key in the SETTINGS
            if self.api_key == SETTINGS.get("api_key"):
                # Log that the user is already logged in
                if verbose:
                    LOGGER.info(f"{PREFIX}Authenticated ✅")
                return
            else:
                # Attempt to authenticate with the provided API key
                success = self.authenticate()
        # If the API key is not provided and the environment is a Google Colab notebook
        elif IS_COLAB:
            # Attempt to authenticate using browser cookies
            success = self.auth_with_cookies()
        else:
            # Request an API key
            success = self.request_api_key()

        # Update SETTINGS with the new API key after successful authentication
        if success:
            SETTINGS.update({"api_key": self.api_key})
            # Log that the new login was successful
            if verbose:
                LOGGER.info(f"{PREFIX}New authentication successful ✅")
        elif verbose:
            LOGGER.info(f"{PREFIX}Get API key from {API_KEY_URL} and then run 'yolo login API_KEY'")
```

Example 4 (python):
```python
def auth_with_cookies(self) -> bool
```

---

## Reference for hub_sdk/base/api_client.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/base/api_client/

**Contents:**
- Reference for hub_sdk/base/api_client.py
- class hub_sdk.base.api_client.APIClientError
  - method hub_sdk.base.api_client.APIClientError.__str__
- class hub_sdk.base.api_client.APIClient
  - method hub_sdk.base.api_client.APIClient._make_request
  - method hub_sdk.base.api_client.APIClient.delete
  - method hub_sdk.base.api_client.APIClient.get
  - method hub_sdk.base.api_client.APIClient.patch
  - method hub_sdk.base.api_client.APIClient.post
  - method hub_sdk.base.api_client.APIClient.put

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/base/api_client.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Custom exception class for API client errors.

Return a string representation of the APIClientError instance.

Represents an API client for making requests to a specified base URL.

Make an HTTP request to the API.

Make a DELETE request to the API.

Make a GET request to the API.

Make a PATCH request to the API.

Make a POST request to the API.

Make a PUT request to the API.

**Examples:**

Example 1 (rust):
```rust
APIClientError(self, message: str, status_code: int | None = None)
```

Example 2 (python):
```python
class APIClientError(Exception):
    """Custom exception class for API client errors.

    Attributes:
        message (str): A human-readable error message.
        status_code (int, optional): The HTTP status code associated with the error, if available.
    """

    def __init__(self, message: str, status_code: int | None = None):
        """Initialize the APIClientError instance.

        Args:
            message (str): A human-readable error message.
            status_code (int, optional): The HTTP status code associated with the error.
        """
        super().__init__(message)
        self.status_code = status_code
        self.message = message
```

Example 3 (python):
```python
def __str__(self) -> str
```

Example 4 (python):
```python
def __str__(self) -> str:
    """Return a string representation of the APIClientError instance."""
    return f"{self.__class__.__name__}: {self.args[0]}"
```

---

## Reference for ultralytics/nn/modules/transformer.py

**URL:** https://docs.ultralytics.com/zh/reference/nn/modules/transformer/

**Contents:**
- Reference for ultralytics/nn/modules/transformer.py
- class ultralytics.nn.modules.transformer.TransformerEncoderLayer
  - method ultralytics.nn.modules.transformer.TransformerEncoderLayer.forward
  - method ultralytics.nn.modules.transformer.TransformerEncoderLayer.forward_post
  - method ultralytics.nn.modules.transformer.TransformerEncoderLayer.forward_pre
  - method ultralytics.nn.modules.transformer.TransformerEncoderLayer.with_pos_embed
- class ultralytics.nn.modules.transformer.AIFI
  - method ultralytics.nn.modules.transformer.AIFI.build_2d_sincos_position_embedding
  - method ultralytics.nn.modules.transformer.AIFI.forward
- class ultralytics.nn.modules.transformer.TransformerLayer

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/nn/modules/transformer.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A single layer of the transformer encoder.

This class implements a standard transformer encoder layer with multi-head attention and feedforward network, supporting both pre-normalization and post-normalization configurations.

Forward propagate the input through the encoder module.

Perform forward pass with post-normalization.

Perform forward pass with pre-normalization.

Add position embeddings to the tensor if provided.

Bases: TransformerEncoderLayer

AIFI transformer layer for 2D data with positional embeddings.

This class extends TransformerEncoderLayer to work with 2D feature maps by adding 2D sine-cosine positional embeddings and handling the spatial dimensions appropriately.

Build 2D sine-cosine position embedding.

Forward pass for the AIFI transformer layer.

Transformer layer https://arxiv.org/abs/2010.11929 (LayerNorm layers removed for better performance).

Apply a transformer block to the input x and return the output.

Vision Transformer block based on https://arxiv.org/abs/2010.11929.

This class implements a complete transformer block with optional convolution layer for channel adjustment, learnable position embedding, and multiple transformer layers.

Forward propagate the input through the transformer block.

A single block of a multi-layer perceptron.

Forward pass for the MLPBlock.

A simple multi-layer perceptron (also called FFN).

This class implements a configurable MLP with multiple linear layers, activation functions, and optional sigmoid output activation.

Forward pass for the entire MLP.

2D Layer Normalization module inspired by Detectron2 and ConvNeXt implementations.

This class implements layer normalization for 2D feature maps, normalizing across the channel dimension while preserving spatial dimensions.

Perform forward pass for 2D layer normalization.

Multiscale Deformable Attention Module based on Deformable-DETR and PaddleDetection implementations.

This module implements multiscale deformable attention that can attend to features at multiple scales with learnable sampling locations and attention weights.

Reset module parameters.

Perform forward pass for multiscale deformable attention.

Deformable Transformer Decoder Layer inspired by PaddleDetection and Deformable-DETR implementations.

This class implements a single decoder layer with self-attention, cross-attention using multiscale deformable attention, and a feedforward network.

Perform the forward pass through the entire decoder layer.

Perform forward pass through the Feed-Forward Network part of the layer.

Add positional embeddings to the input tensor, if provided.

Deformable Transformer Decoder based on PaddleDetection implementation.

This class implements a complete deformable transformer decoder with multiple decoder layers and prediction heads for bounding box regression and classification.

Perform the forward pass through the entire decoder.

**Examples:**

Example 1 (python):
```python
def __init__(
    self,
    c1: int,
    cm: int = 2048,
    num_heads: int = 8,
    dropout: float = 0.0,
    act: nn.Module = nn.GELU(),
    normalize_before: bool = False,
)
```

Example 2 (python):
```python
class TransformerEncoderLayer(nn.Module):
    """A single layer of the transformer encoder.

    This class implements a standard transformer encoder layer with multi-head attention and feedforward network,
    supporting both pre-normalization and post-normalization configurations.

    Attributes:
        ma (nn.MultiheadAttention): Multi-head attention module.
        fc1 (nn.Linear): First linear layer in the feedforward network.
        fc2 (nn.Linear): Second linear layer in the feedforward network.
        norm1 (nn.LayerNorm): Layer normalization after attention.
        norm2 (nn.LayerNorm): Layer normalization after feedforward network.
        dropout (nn.Dropout): Dropout layer for the feedforward network.
        dropout1 (nn.Dropout): Dropout layer after attention.
        dropout2 (nn.Dropout): Dropout layer after feedforward network.
        act (nn.Module): Activation function.
        normalize_before (bool): Whether to apply normalization before attention and feedforward.
    """

    def __init__(
        self,
        c1: int,
        cm: int = 2048,
        num_heads: int = 8,
        dropout: float = 0.0,
        act: nn.Module = nn.GELU(),
        normalize_before: bool = False,
    ):
        """Initialize the TransformerEncoderLayer with specified parameters.

        Args:
            c1 (int): Input dimension.
            cm (int): Hidden dimension in the feedforward network.
            num_heads (int): Number of attention heads.
            dropout (float): Dropout probability.
            act (nn.Module): Activation function.
            normalize_before (bool): Whether to apply normalization before attention and feedforward.
        """
        super().__init__()
        from ...utils.torch_utils import TORCH_1_9

        if not TORCH_1_9:
            raise ModuleNotFoundError(
                "TransformerEncoderLayer() requires torch>=1.9 to use nn.MultiheadAttention(batch_first=True)."
            )
        self.ma = nn.MultiheadAttention(c1, num_heads, dropout=dropout, batch_first=True)
        # Implementation of Feedforward model
        self.fc1 = nn.Linear(c1, cm)
        self.fc2 = nn.Linear(cm, c1)

        self.norm1 = nn.LayerNorm(c1)
        self.norm2 = nn.LayerNorm(c1)
        self.dropout = nn.Dropout(dropout)
        self.dropout1 = nn.Dropout(dropout)
        self.dropout2 = nn.Dropout(dropout)

        self.act = act
        self.normalize_before = normalize_before
```

Example 3 (python):
```python
def forward(
    self,
    src: torch.Tensor,
    src_mask: torch.Tensor | None = None,
    src_key_padding_mask: torch.Tensor | None = None,
    pos: torch.Tensor | None = None,
) -> torch.Tensor
```

Example 4 (python):
```python
def forward(
    self,
    src: torch.Tensor,
    src_mask: torch.Tensor | None = None,
    src_key_padding_mask: torch.Tensor | None = None,
    pos: torch.Tensor | None = None,
) -> torch.Tensor:
    """Forward propagate the input through the encoder module.

    Args:
        src (torch.Tensor): Input tensor.
        src_mask (torch.Tensor, optional): Mask for the src sequence.
        src_key_padding_mask (torch.Tensor, optional): Mask for the src keys per batch.
        pos (torch.Tensor, optional): Positional encoding.

    Returns:
        (torch.Tensor): Output tensor after transformer encoder layer.
    """
    if self.normalize_before:
        return self.forward_pre(src, src_mask, src_key_padding_mask, pos)
    return self.forward_post(src, src_mask, src_key_padding_mask, pos)
```

---

## Reference for hub_sdk/modules/projects.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/modules/projects/

**Contents:**
- Reference for hub_sdk/modules/projects.py
- class hub_sdk.modules.projects.Projects
  - method hub_sdk.modules.projects.Projects.create_project
  - method hub_sdk.modules.projects.Projects.delete
  - method hub_sdk.modules.projects.Projects.get_data
  - method hub_sdk.modules.projects.Projects.update
  - method hub_sdk.modules.projects.Projects.upload_image
- class hub_sdk.modules.projects.ProjectList

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/modules/projects.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class representing a client for interacting with Projects through CRUD operations.

This class extends the CRUDClient class and provides specific methods for working with Projects.

The 'id' attribute is set during initialization and can be used to uniquely identify a project. The 'data' attribute is used to store project data fetched from the API.

Create a new project with the provided data and set the project ID for the current instance.

Delete the project resource represented by this instance.

The 'hard' parameter determines whether to perform a soft delete (default) or a hard delete. In a soft delete, the project might be marked as deleted but retained in the system. In a hard delete, the project is permanently removed from the system.

Retrieve data for the current project instance.

If a valid project ID has been set, it sends a request to fetch the project data and stores it in the instance. If no project ID has been set, it logs an error message.

Update the project resource represented by this instance.

Upload an image file to the hub associated with this client.

Provides a paginated list interface for querying project resources from the server.

**Examples:**

Example 1 (rust):
```rust
Projects(self, project_id: str | None = None, headers: dict[str, Any] | None = None)
```

Example 2 (python):
```python
class Projects(CRUDClient):
    """A class representing a client for interacting with Projects through CRUD operations.

    This class extends the CRUDClient class and provides specific methods for working with Projects.

    Attributes:
        hub_client (ProjectUpload): An instance of ProjectUpload used for interacting with model uploads.
        id (str | None): The unique identifier of the project, if available.
        data (Dict): A dictionary to store project data.

    Notes:
        The 'id' attribute is set during initialization and can be used to uniquely identify a project.
        The 'data' attribute is used to store project data fetched from the API.
    """

    def __init__(self, project_id: str | None = None, headers: dict[str, Any] | None = None):
        """Initialize a Projects object for interacting with project data via CRUD operations.

        Args:
            project_id (str, optional): Project ID for retrieving data.
            headers (Dict[str, Any], optional): A dictionary of HTTP headers to be included in API requests.
        """
        super().__init__("projects", "project", headers)
        self.hub_client = ProjectUpload(headers)
        self.id = project_id
        self.data = {}
        if project_id:
            self.get_data()
```

Example 3 (python):
```python
def create_project(self, project_data: dict) -> None
```

Example 4 (python):
```python
def create_project(self, project_data: dict) -> None:
    """Create a new project with the provided data and set the project ID for the current instance.

    Args:
        project_data (Dict): A dictionary containing the data for creating the project.
    """
    resp = super().create(project_data).json()
    self.id = resp.get("data", {}).get("id")
    self.get_data()
```

---

## Ultralytics HUB 团队 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/hub/teams/

**Contents:**
- Ultralytics HUB 团队
- 创建团队
- 编辑团队
- 删除团队
- 邀请成员
  - 座位
- 移除成员
- 加入团队
- 离开团队
- 分享数据集

我们很高兴向 Pro 用户介绍 Ultralytics HUB 中的新团队功能！

在这里，您将学习如何管理团队成员、无缝共享资源以及在各种项目上高效协作。

由于这是一项新功能，我们仍在开发和完善它，以确保它满足您的需求。

导航到 Teams 页面，打开 Settings 页面中的团队操作下拉菜单，然后单击 Create Team 按钮。

在团队名称字段中键入您的团队名称，或保留默认名称，然后单击一下即可完成团队创建。

您可以选择使用描述和独特的图像来丰富您的团队，从而提高其在团队页面上的可识别性。

您的团队创建后，您将能够从Teams页面访问它。

导航到 Teams 页面，打开要编辑的团队的团队操作下拉菜单，然后单击 Edit 选项。此操作将触发 Update Team 对话框。

对您的团队应用所需的修改，然后单击 保存 以确认更改。

导航到 Teams 页面，打开要删除的团队的团队操作下拉菜单，然后单击 Delete 选项。

导航至您想要添加新成员的团队的团队页面，然后点击 邀请成员 按钮。此操作将触发 邀请成员 对话框。

输入电子邮件，选择新成员的角色，然后点击邀请。

管理员角色允许邀请和移除成员，以及移除共享数据集或项目。

Pro Plan提供一个免费席位(您自己的)。

当一个新的唯一成员加入您的团队时，席位数将增加，并且您将为每个席位支付每月 20 美元，或者如果您选择年度计划，则支付每年 200 美元。

每个唯一成员计为一个席位，无论他们属于多少个团队。例如，如果 John Doe 是您 5 个团队的成员，他只占用一个席位。

当您从他们所属的最后一个团队中删除一个唯一成员时，席位数会减少。费用按比例计算，可用于添加其他唯一成员、支付 Pro Plan 或充值您的 账户余额。

导航至您想要从中移除成员的团队的团队页面，打开成员操作下拉菜单，然后点击 移除 选项。

当您被邀请加入团队时，您会收到一个应用内通知。

您可以通过单击 主页页面上通知卡上的查看按钮来查看您的通知。

或者，您可以通过直接访问通知页面来查看您的通知。

您可以决定是否加入您受邀加入的团队的“团队”页面。

如果您想加入团队，请单击 加入团队 按钮。

如果您不想加入团队，请点击 拒绝邀请 按钮。

导航至您想要离开的团队的团队页面，然后点击 离开团队 按钮。

导航至您想要与之共享数据集的团队的团队页面，然后点击 添加数据集 按钮。

选择您想要与团队分享的数据集，然后点击 添加 按钮。

就这样！您的团队现在可以访问您的数据集了。

作为团队所有者或团队管理员，您可以删除共享数据集。

导航至您想要与之共享项目的团队的团队页面，然后点击 添加项目 按钮。

选择您想要与团队分享的项目，然后点击 添加 按钮。

作为团队所有者或团队管理员，您可以删除共享项目。

当您与您的团队共享一个项目时，该项目中的所有模型也会被共享。

---

## Reference for hub_sdk/modules/teams.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/modules/teams/

**Contents:**
- Reference for hub_sdk/modules/teams.py
- class hub_sdk.modules.teams.Teams
  - method hub_sdk.modules.teams.Teams.create_team
  - method hub_sdk.modules.teams.Teams.delete
  - method hub_sdk.modules.teams.Teams.get_data
  - method hub_sdk.modules.teams.Teams.update
- class hub_sdk.modules.teams.TeamList

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/modules/teams.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class representing a client for interacting with Teams through CRUD operations.

This class extends the CRUDClient class and provides specific methods for working with Teams.

The 'id' attribute is set during initialization and can be used to uniquely identify a team. The 'data' attribute is used to store team data fetched from the API.

Create a new team with the provided data and set the team ID for the current instance.

Delete the team resource represented by this instance.

The 'hard' parameter determines whether to perform a soft delete (default) or a hard delete. In a soft delete, the team might be marked as deleted but retained in the system. In a hard delete, the team is permanently removed from the system.

Retrieve data for the current team instance.

If a valid team ID has been set, it sends a request to fetch the team data and stores it in the instance. If no team ID has been set, it logs an error message.

Update the team resource represented by this instance.

Provides a paginated list interface for managing and retrieving teams via API requests.

**Examples:**

Example 1 (rust):
```rust
Teams(self, team_id: str | None = None, headers: dict[str, Any] | None = None)
```

Example 2 (python):
```python
class Teams(CRUDClient):
    """A class representing a client for interacting with Teams through CRUD operations.

    This class extends the CRUDClient class and provides specific methods for working with Teams.

    Attributes:
        id (str | None): The unique identifier of the team, if available.
        data (Dict): A dictionary to store team data.

    Notes:
        The 'id' attribute is set during initialization and can be used to uniquely identify a team.
        The 'data' attribute is used to store team data fetched from the API.
    """

    def __init__(self, team_id: str | None = None, headers: dict[str, Any] | None = None):
        """Initialize a Teams instance.

        Args:
            team_id (str, optional): The unique identifier of the team.
            headers (Dict[str, Any], optional): A dictionary of HTTP headers to be included in API requests.
        """
        super().__init__("teams", "team", headers)
        self.id = team_id
        self.data = {}
        if team_id:
            self.get_data()
```

Example 3 (python):
```python
def create_team(self, team_data: dict[str, Any]) -> None
```

Example 4 (python):
```python
def create_team(self, team_data: dict[str, Any]) -> None:
    """Create a new team with the provided data and set the team ID for the current instance.

    Args:
        team_data (Dict[str, Any]): A dictionary containing the data for creating the team.
    """
    resp = super().create(team_data).json()
    self.id = resp.get("data", {}).get("id")
    self.get_data()
```

---

## Reference for ultralytics/solutions/similarity_search.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/similarity_search/

**Contents:**
- Reference for ultralytics/solutions/similarity_search.py
- class ultralytics.solutions.similarity_search.VisualAISearch
  - method ultralytics.solutions.similarity_search.VisualAISearch.__call__
  - method ultralytics.solutions.similarity_search.VisualAISearch.extract_image_feature
  - method ultralytics.solutions.similarity_search.VisualAISearch.extract_text_feature
  - method ultralytics.solutions.similarity_search.VisualAISearch.load_or_build_index
  - method ultralytics.solutions.similarity_search.VisualAISearch.search
- class ultralytics.solutions.similarity_search.SearchApp
  - method ultralytics.solutions.similarity_search.SearchApp.index
  - method ultralytics.solutions.similarity_search.SearchApp.run

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/similarity_search.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A semantic image search system that leverages OpenCLIP for generating high-quality image and text embeddings and

FAISS for fast similarity-based retrieval.

This class aligns image and text embeddings in a shared semantic space, enabling users to search large collections of images using natural language queries with high accuracy and speed.

Direct call interface for the search function.

Extract CLIP image embedding from the given image path.

Extract CLIP text embedding from the given text query.

Load existing FAISS index or build a new one from image features.

Checks if FAISS index and image paths exist on disk. If found, loads them directly. Otherwise, builds a new index by extracting features from all images in the data directory, normalizes the features, and saves both the index and image paths for future use.

Return top-k semantically similar images to the given query.

A Flask-based web interface for semantic image search with natural language queries.

This class provides a clean, responsive frontend that enables users to input natural language queries and instantly view the most relevant images retrieved from the indexed database.

Process user query and display search results in the web interface.

Start the Flask web application server.

**Examples:**

Example 1 (rust):
```rust
VisualAISearch(self, **kwargs: Any) -> None
```

Example 2 (unknown):
```unknown
Initialize and search for images
>>> searcher = VisualAISearch(data="path/to/images", device="cuda")
>>> results = searcher.search("a cat sitting on a chair", k=10)
```

Example 3 (python):
```python
class VisualAISearch:
    """A semantic image search system that leverages OpenCLIP for generating high-quality image and text embeddings and
    FAISS for fast similarity-based retrieval.

    This class aligns image and text embeddings in a shared semantic space, enabling users to search large collections
    of images using natural language queries with high accuracy and speed.

    Attributes:
        data (str): Directory containing images.
        device (str): Computation device, e.g., 'cpu' or 'cuda'.
        faiss_index (str): Path to the FAISS index file.
        data_path_npy (str): Path to the numpy file storing image paths.
        data_dir (Path): Path object for the data directory.
        model: Loaded CLIP model.
        index: FAISS index for similarity search.
        image_paths (list[str]): List of image file paths.

    Methods:
        extract_image_feature: Extract CLIP embedding from an image.
        extract_text_feature: Extract CLIP embedding from text.
        load_or_build_index: Load existing FAISS index or build new one.
        search: Perform semantic search for similar images.

    Examples:
        Initialize and search for images
        >>> searcher = VisualAISearch(data="path/to/images", device="cuda")
        >>> results = searcher.search("a cat sitting on a chair", k=10)
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the VisualAISearch class with FAISS index and CLIP model."""
        assert TORCH_2_4, f"VisualAISearch requires torch>=2.4 (found torch=={TORCH_VERSION})"
        from ultralytics.nn.text_model import build_text_model

        check_requirements("faiss-cpu")

        self.faiss = __import__("faiss")
        self.faiss_index = "faiss.index"
        self.data_path_npy = "paths.npy"
        self.data_dir = Path(kwargs.get("data", "images"))
        self.device = select_device(kwargs.get("device", "cpu"))

        if not self.data_dir.exists():
            from ultralytics.utils import ASSETS_URL

            LOGGER.warning(f"{self.data_dir} not found. Downloading images.zip from {ASSETS_URL}/images.zip")
            from ultralytics.utils.downloads import safe_download

            safe_download(url=f"{ASSETS_URL}/images.zip", unzip=True, retry=3)
            self.data_dir = Path("images")

        self.model = build_text_model("clip:ViT-B/32", device=self.device)

        self.index = None
        self.image_paths = []

        self.load_or_build_index()
```

Example 4 (python):
```python
def __call__(self, query: str) -> list[str]
```

---

## 用于 Ultralytics YOLO 的 MLflow 集成 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/integrations/mlflow/

**Contents:**
- Ultralytics YOLO 的 MLflow 集成
- 简介
- 什么是 MLflow？
- 功能
- 设置和先决条件
- 如何使用
  - 命令
  - 日志记录
- 示例
- 禁用 MLflow

实验日志记录是 机器学习 工作流程的一个关键方面，它可以跟踪各种指标、参数和工件。它有助于提高模型的可重复性、调试问题和提高模型性能。以其实时对象检测功能而闻名的 Ultralytics YOLO 现在提供与 MLflow 的集成，MLflow 是一个用于完整机器学习生命周期管理的开源平台。

此文档页面是关于为您的 Ultralytics YOLO 项目设置和使用 MLflow 日志记录功能的综合指南。

MLflow 是由 Databricks 开发的开源平台，用于管理端到端的机器学习生命周期。它包括用于跟踪实验、将代码打包到可重现的运行中以及共享和部署模型的工具。MLflow 旨在与任何机器学习库和编程语言配合使用。

确保已安装 MLflow。 如果没有，请使用 pip 安装：

确保在 Ultralytics 设置中启用了 MLflow 日志记录。通常，这由设置控制。 mlflow 键。请参见 设置 页面以获取更多信息。

更新 Ultralytics MLflow 设置

在 Python 环境中，调用 update 上的 settings 方法来更改您的设置：

如果您喜欢使用命令行界面，以下命令将允许您修改您的设置：

设置项目名称: 您可以通过环境变量设置项目名称：

或使用 project=<project> 训练 YOLO 模型时的参数，即 yolo train project=my_project.

设置运行名称: 与设置项目名称类似，您可以通过环境变量设置运行名称：

或使用 name=<name> 训练 YOLO 模型时的参数，即 yolo train project=my_project name=my_name.

启动本地 MLflow 服务器：要开始跟踪，请使用：

这将在以下位置启动本地服务器： http://127.0.0.1:5000 默认情况下，会将所有 mlflow 日志保存到“runs/mlflow”目录。要指定其他 URI，请设置 MLFLOW_TRACKING_URI 环境变量。

终止 MLflow 服务器实例：要停止所有正在运行的 MLflow 实例，请运行：

日志记录由以下对象负责 on_pretrain_routine_end, on_fit_epoch_end和 on_train_end 回调函数。这些函数在训练过程的各个阶段自动调用，并处理参数、指标和 artifacts 的日志记录。

记录自定义指标：您可以通过修改以下内容来添加要记录的自定义指标 trainer.metrics 之前的字典 on_fit_epoch_end 被调用。

查看实验：要查看您的日志，请导航到您的 MLflow 服务器（通常 http://127.0.0.1:5000)，然后选择你的实验并运行。

查看运行：Runs 是实验中的单个模型。点击 Run 可以查看 Run 详细信息，包括上传的 artifacts 和模型权重。

Ultralytics YOLO 与 MLflow 日志记录集成提供了一种简化方式来跟踪您的 机器学习实验。它使您能够有效地监控性能指标和管理工件，从而有助于稳健的模型开发和部署。有关更多详细信息，请访问 MLflow 官方文档。

要设置 Ultralytics YOLO 的 MLflow 日志记录，首先需要确保已安装 MLflow。您可以使用 pip 安装它：

接下来，在 Ultralytics 设置中启用 MLflow 日志记录。这可以使用以下方式进行控制 mlflow 键。更多信息，请参见 设置指南.

更新 Ultralytics MLflow 设置

最后，启动一个本地 MLflow 服务器进行跟踪：

Ultralytics YOLO 与 MLflow 结合使用，支持记录整个训练过程中的各种指标、参数和工件：

有关更多详细信息，请访问 Ultralytics YOLO 跟踪文档。

是的，您可以通过更新设置来禁用 Ultralytics YOLO 的 MLflow 日志记录。以下是使用 CLI 执行此操作的方法：

有关进一步的自定义和重置设置，请参阅设置指南。

要启动 MLflow 服务器以跟踪 Ultralytics YOLO 中的实验，请使用以下命令：

此命令在以下位置启动本地服务器： http://127.0.0.1:5000 默认情况下。 如果您需要停止运行 MLflow 服务器实例，请使用以下 bash 命令：

将 MLflow 与 Ultralytics YOLO 集成，为管理您的机器学习实验带来诸多好处：

要深入了解如何使用 Ultralytics YOLO 设置和利用 MLflow，请查阅Ultralytics YOLO 的 MLflow 集成文档。

**Examples:**

Example 1 (unknown):
```unknown
pip install mlflow
```

Example 2 (sql):
```sql
from ultralytics import settings

# Update a setting
settings.update({"mlflow": True})

# Reset settings to default values
settings.reset()
```

Example 3 (sql):
```sql
# Update a setting
yolo settings mlflow=True

# Reset settings to default values
yolo settings reset
```

Example 4 (unknown):
```unknown
export MLFLOW_EXPERIMENT_NAME=YOUR_EXPERIMENT_NAME
```

---

## Reference for ultralytics/solutions/ai_gym.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/solutions/ai_gym/

**Contents:**
- Reference for ultralytics/solutions/ai_gym.py
- class ultralytics.solutions.ai_gym.AIGym
  - method ultralytics.solutions.ai_gym.AIGym.process

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/ai_gym.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class to manage gym steps of people in a real-time video stream based on their poses.

This class extends BaseSolution to monitor workouts using YOLO pose estimation models. It tracks and counts repetitions of exercises based on predefined angle thresholds for up and down positions.

Monitor workouts using Ultralytics YOLO Pose Model.

This function processes an input image to track and analyze human poses for workout monitoring. It uses the YOLO Pose model to detect keypoints, estimate angles, and count repetitions based on predefined angle thresholds.

**Examples:**

Example 1 (rust):
```rust
AIGym(self, **kwargs: Any) -> None
```

Example 2 (unknown):
```unknown
>>> gym = AIGym(model="yolo11n-pose.pt")
>>> image = cv2.imread("gym_scene.jpg")
>>> results = gym.process(image)
>>> processed_image = results.plot_im
>>> cv2.imshow("Processed Image", processed_image)
>>> cv2.waitKey(0)
```

Example 3 (python):
```python
class AIGym(BaseSolution):
    """A class to manage gym steps of people in a real-time video stream based on their poses.

    This class extends BaseSolution to monitor workouts using YOLO pose estimation models. It tracks and counts
    repetitions of exercises based on predefined angle thresholds for up and down positions.

    Attributes:
        states (dict[int, dict[str, float | int | str]]): Per-track angle, rep count, and stage for workout monitoring.
        up_angle (float): Angle threshold for considering the 'up' position of an exercise.
        down_angle (float): Angle threshold for considering the 'down' position of an exercise.
        kpts (list[int]): Indices of keypoints used for angle calculation.

    Methods:
        process: Process a frame to detect poses, calculate angles, and count repetitions.

    Examples:
        >>> gym = AIGym(model="yolo11n-pose.pt")
        >>> image = cv2.imread("gym_scene.jpg")
        >>> results = gym.process(image)
        >>> processed_image = results.plot_im
        >>> cv2.imshow("Processed Image", processed_image)
        >>> cv2.waitKey(0)
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize AIGym for workout monitoring using pose estimation and predefined angles.

        Args:
            **kwargs (Any): Keyword arguments passed to the parent class constructor including:
                - model (str): Model name or path, defaults to "yolo11n-pose.pt".
        """
        kwargs["model"] = kwargs.get("model", "yolo11n-pose.pt")
        super().__init__(**kwargs)
        self.states = defaultdict(lambda: {"angle": 0, "count": 0, "stage": "-"})  # Dict for count, angle and stage

        # Extract details from CFG single time for usage later
        self.up_angle = float(self.CFG["up_angle"])  # Pose up predefined angle to consider up pose
        self.down_angle = float(self.CFG["down_angle"])  # Pose down predefined angle to consider down pose
        self.kpts = self.CFG["kpts"]  # User selected kpts of workouts storage for further usage
```

Example 4 (python):
```python
def process(self, im0) -> SolutionResults
```

---

## Ultralytics 贡献者公约行为准则

**URL:** https://docs.ultralytics.com/zh/help/code-of-conduct/

**Contents:**
- Ultralytics 贡献者公约行为准则
- 我们的承诺
- 我们的标准
- 执行责任
- 范围
- 执行
- 执行指南
  - 1. 更正
  - 2. 警告
  - 3. 临时禁令

我们作为成员、贡献者和领导者承诺，无论年龄、体型、可见或不可见的残疾、种族、性别特征、性别认同和表达、经验水平、教育程度、社会经济地位、国籍、个人外貌、种族、宗教或性认同和性取向如何，我们社区的参与者都将获得免受骚扰的体验。

我们承诺以有助于建立开放、热情、多元、包容和健康的社区的方式行事和互动。

有助于为我们的社区创造积极环境的行为示例包括：

社区领导负责澄清和执行我们可接受行为的标准，并将采取适当和公正的纠正措施，以回应他们认为不适当、具有威胁性、冒犯性或有害的任何行为。

社区领导有权力和责任删除、编辑或拒绝与本行为准则不符的评论、提交、代码、Wiki 编辑、问题和其他贡献，并在适当时沟通采取审核决定的原因。

本行为准则适用于所有社区空间，也适用于个人在公共空间正式代表社区时。代表我们社区的例子包括使用官方电子邮件地址、通过官方社交媒体帐户发布信息，或在在线或线下活动中担任指定的代表。

虐待、骚扰或其他不可接受行为的实例可以报告给负责执行的社区领导者，邮箱地址为 hello@ultralytics.com。所有投诉都将得到及时公正的审查和调查。

所有社区领导者都有义务尊重任何事件举报者的隐私和安全。

社区领导将遵循这些社区影响指南，以确定他们认为违反本行为准则的任何行为的后果：

社区影响 (Community Impact): 在社区中使用不当语言或其他被认为不专业或不受欢迎的行为。

后果: 社区领导者给予私下书面警告，明确说明违规行为的性质，并解释该行为为何不当。可能会要求公开道歉。

社区影响 (Community Impact): 通过单一事件或一系列行为造成的违规。

后果: 警告，并对持续行为产生后果。在指定的时间段内，不得与相关人员进行任何互动，包括主动与执行行为准则的人员进行互动。这包括避免在社区空间以及社交媒体等外部渠道中进行互动。违反这些条款可能会导致暂时或永久禁令。

社区影响 (Community Impact): 严重违反社区标准，包括持续的不当行为。

后果: 在指定的时间段内，暂时禁止与社区进行任何形式的互动或公开交流。在此期间，不允许与相关人员进行任何公开或私下互动，包括主动与执行行为准则的人员进行互动。违反这些条款可能会导致永久禁令。

社区影响 (Community Impact): 表现出违反社区标准的模式，包括持续的不当行为、骚扰个人或攻击或贬低某类人。

后果: 永久禁止在社区内进行任何形式的公开互动。

本行为准则改编自《贡献者契约》2.0版，该版本详见《贡献者契约》行为准则。

社区影响指南的灵感来自Mozilla 的行为准则执行阶梯。

有关本行为准则的常见问题解答，请参阅《贡献者契约》常见问题解答。翻译版本可在《贡献者契约》翻译页面获取。

Ultralytics 贡献者公约行为准则旨在为参与 Ultralytics 社区的每个人创造一个免受骚扰的体验。它适用于所有社区互动，包括线上和线下活动。该准则详细说明了预期行为、不可接受的行为以及社区领导者的执行责任。有关更多详细信息，请参见执行责任部分。

Ultralytics 行为准则的执行由社区领导管理，他们可以对任何被认为不当的行为采取适当的行动。 这可能包括私人警告到永久禁令，具体取决于违规的严重程度。 不当行为的实例可以报告给 hello@ultralytics.com 进行调查。 在执行指南部分中了解有关执行步骤的更多信息。

Ultralytics 认为多样性和包容性是促进社区内创新和创造力的根本要素。一个多元化和包容性的环境能够让不同的观点和经验为开放、热情和健康的社区做出贡献。我们承诺确保每个人都能在免受骚扰的环境中体验，这体现在我们的 Pledge 中。

为 Ultralytics 做出贡献意味着以积极和尊重的态度与其他社区成员互动。你可以通过展示同理心、提供和接受建设性的反馈，以及对任何错误负责来做出贡献。始终致力于以一种有益于整个社区的方式做出贡献。有关可接受行为的更多详细信息，请参阅我们的标准部分。

有关 Ultralytics 行为准则的更全面详细信息，包括报告指南和执行政策，您可以访问贡献者盟约主页或查看贡献者盟约的常见问题解答部分。在我们的品牌页面和关于页面上了解有关 Ultralytics 目标和举措的更多信息。

如果您有更多问题或需要进一步的帮助，请查看我们的帮助中心和贡献指南以获取更多信息。

---

## Reference for hub_sdk/base/crud_client.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/base/crud_client/

**Contents:**
- Reference for hub_sdk/base/crud_client.py
- class hub_sdk.base.crud_client.CRUDClient
  - method hub_sdk.base.crud_client.CRUDClient.create
  - method hub_sdk.base.crud_client.CRUDClient.delete
  - method hub_sdk.base.crud_client.CRUDClient.list
  - method hub_sdk.base.crud_client.CRUDClient.read
  - method hub_sdk.base.crud_client.CRUDClient.update

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/base/crud_client.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Represents a CRUD (Create, Read, Update, Delete) client for interacting with a specific resource.

This class provides methods for performing standard CRUD operations on API resources, handling errors gracefully and providing logging for failed operations.

Create a new entity using the API.

Delete an entity using the API.

List entities using the API with pagination support.

Retrieve details of a specific entity.

Update an existing entity using the API.

**Examples:**

Example 1 (unknown):
```unknown
CRUDClient(self, base_endpoint: str, name: str, headers: dict)
```

Example 2 (python):
```python
class CRUDClient(APIClient):
    """Represents a CRUD (Create, Read, Update, Delete) client for interacting with a specific resource.

    This class provides methods for performing standard CRUD operations on API resources, handling errors gracefully and
    providing logging for failed operations.

    Attributes:
        name (str): The name associated with the CRUD operations (e.g., "User").
        logger (logging.Logger): An instance of the logger for logging purposes.
    """

    def __init__(self, base_endpoint: str, name: str, headers: dict):
        """Initialize a CRUDClient instance.

        Args:
            base_endpoint (str): The base endpoint URL for the API.
            name (str): The name associated with the CRUD operations (e.g., "User").
            headers (dict): Headers to be included in API requests.
        """
        super().__init__(f"{HUB_FUNCTIONS_ROOT}/v1/{base_endpoint}", headers)
        self.name = name
        self.logger = logger
```

Example 3 (python):
```python
def create(self, data: dict) -> Response | None
```

Example 4 (python):
```python
def create(self, data: dict) -> Response | None:
    """Create a new entity using the API.

    Args:
        data (dict): The data to be sent as part of the creation request.

    Returns:
        (Optional[Response]): Response object from the create request, or None if creation fails.
    """
    try:
        return self.post("", json=data)
    except Exception as e:
        self.logger.error(f"Failed to create {self.name}: {e}")
```

---

## Reference for ultralytics/nn/autobackend.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/nn/autobackend/

**Contents:**
- Reference for ultralytics/nn/autobackend.py
- class ultralytics.nn.autobackend.AutoBackend
  - method ultralytics.nn.autobackend.AutoBackend._model_type
  - method ultralytics.nn.autobackend.AutoBackend.forward
  - method ultralytics.nn.autobackend.AutoBackend.from_numpy
  - method ultralytics.nn.autobackend.AutoBackend.warmup
- function ultralytics.nn.autobackend.check_class_names
- function ultralytics.nn.autobackend.default_class_names

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/nn/autobackend.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Handle dynamic backend selection for running inference using Ultralytics YOLO models.

The AutoBackend class is designed to provide an abstraction layer for various inference engines. It supports a wide range of formats, each with specific naming conventions as outlined below:

Take a path to a model file and return the model type.

Run inference on an AutoBackend model.

Convert a NumPy array to a torch tensor on the model device.

Warm up the model by running one forward pass with a dummy input.

Check class names and convert to dict format if needed.

Apply default class names to an input YAML file or return numerical class names.

**Examples:**

Example 1 (python):
```python
def __init__(
    self,
    model: str | torch.nn.Module = "yolo11n.pt",
    device: torch.device = torch.device("cpu"),
    dnn: bool = False,
    data: str | Path | None = None,
    fp16: bool = False,
    fuse: bool = True,
    verbose: bool = True,
)
```

Example 2 (yaml):
```yaml
Supported Formats and Naming Conventions:
    | Format                | File Suffix       |
    | --------------------- | ----------------- |
    | PyTorch               | *.pt              |
    | TorchScript           | *.torchscript     |
    | ONNX Runtime          | *.onnx            |
    | ONNX OpenCV DNN       | *.onnx (dnn=True) |
    | OpenVINO              | *openvino_model/  |
    | CoreML                | *.mlpackage       |
    | TensorRT              | *.engine          |
    | TensorFlow SavedModel | *_saved_model/    |
    | TensorFlow GraphDef   | *.pb              |
    | TensorFlow Lite       | *.tflite          |
    | TensorFlow Edge TPU   | *_edgetpu.tflite  |
    | PaddlePaddle          | *_paddle_model/   |
    | MNN                   | *.mnn             |
    | NCNN                  | *_ncnn_model/     |
    | IMX                   | *_imx_model/      |
    | RKNN                  | *_rknn_model/     |
    | Triton Inference      | triton://model    |
    | ExecuTorch            | *.pte             |
    | Axelera               | *_axelera_model/  |
```

Example 3 (unknown):
```unknown
>>> model = AutoBackend(model="yolo11n.pt", device="cuda")
>>> results = model(img)
```

Example 4 (python):
```python
class AutoBackend(nn.Module):
    """Handle dynamic backend selection for running inference using Ultralytics YOLO models.

    The AutoBackend class is designed to provide an abstraction layer for various inference engines. It supports a wide
    range of formats, each with specific naming conventions as outlined below:

        Supported Formats and Naming Conventions:
            | Format                | File Suffix       |
            | --------------------- | ----------------- |
            | PyTorch               | *.pt              |
            | TorchScript           | *.torchscript     |
            | ONNX Runtime          | *.onnx            |
            | ONNX OpenCV DNN       | *.onnx (dnn=True) |
            | OpenVINO              | *openvino_model/  |
            | CoreML                | *.mlpackage       |
            | TensorRT              | *.engine          |
            | TensorFlow SavedModel | *_saved_model/    |
            | TensorFlow GraphDef   | *.pb              |
            | TensorFlow Lite       | *.tflite          |
            | TensorFlow Edge TPU   | *_edgetpu.tflite  |
            | PaddlePaddle          | *_paddle_model/   |
            | MNN                   | *.mnn             |
            | NCNN                  | *_ncnn_model/     |
            | IMX                   | *_imx_model/      |
            | RKNN                  | *_rknn_model/     |
            | Triton Inference      | triton://model    |
            | ExecuTorch            | *.pte             |
            | Axelera               | *_axelera_model/  |

    Attributes:
        model (torch.nn.Module): The loaded YOLO model.
        device (torch.device): The device (CPU or GPU) on which the model is loaded.
        task (str): The type of task the model performs (detect, segment, classify, pose).
        names (dict): A dictionary of class names that the model can detect.
        stride (int): The model stride, typically 32 for YOLO models.
        fp16 (bool): Whether the model uses half-precision (FP16) inference.
        nhwc (bool): Whether the model expects NHWC input format instead of NCHW.
        pt (bool): Whether the model is a PyTorch model.
        jit (bool): Whether the model is a TorchScript model.
        onnx (bool): Whether the model is an ONNX model.
        xml (bool): Whether the model is an OpenVINO model.
        engine (bool): Whether the model is a TensorRT engine.
        coreml (bool): Whether the model is a CoreML model.
        saved_model (bool): Whether the model is a TensorFlow SavedModel.
        pb (bool): Whether the model is a TensorFlow GraphDef.
        tflite (bool): Whether the model is a TensorFlow Lite model.
        edgetpu (bool): Whether the model is a TensorFlow Edge TPU model.
        tfjs (bool): Whether the model is a TensorFlow.js model.
        paddle (bool): Whether the model is a PaddlePaddle model.
        mnn (bool): Whether the model is an MNN model.
        ncnn (bool): Whether the model is an NCNN model.
        imx (bool): Whether the model is an IMX model.
        rknn (bool): Whether the model is an RKNN model.
        triton (bool): Whether the model is a Triton Inference Server model.
        pte (bool): Whether the model is a PyTorch ExecuTorch model.
        axelera (bool): Whether the model is an Axelera model.

    Methods:
        forward: Run inference on an input image.
        from_numpy: Convert NumPy arrays to tensors on the model device.
        warmup: Warm up the model with a dummy input.
        _model_type: Determine the model type from file path.

    Examples:
        >>> model = AutoBackend(model="yolo11n.pt", device="cuda")
        >>> results = model(img)
    """

    @torch.no_grad()
    def __init__(
        self,
        model: str | torch.nn.Module = "yolo11n.pt",
        device: torch.device = torch.device("cpu"),
        dnn: bool = False,
        data: str | Path | None = None,
        fp16: bool = False,
        fuse: bool = True,
        verbose: bool = True,
    ):
        """Initialize the AutoBackend for inference.

        Args:
            model (str | torch.nn.Module): Path to the model weights file or a module instance.
            device (torch.device): Device to run the model on.
            dnn (bool): Use OpenCV DNN module for ONNX inference.
            data (str | Path, optional): Path to the additional data.yaml file containing class names.
            fp16 (bool): Enable half-precision inference. Supported only on specific backends.
            fuse (bool): Fuse Conv2D + BatchNorm layers for optimization.
            verbose (bool): Enable verbose logging.
        """
        super().__init__()
        nn_module = isinstance(model, torch.nn.Module)
        (
            pt,
            jit,
            onnx,
            xml,
            engine,
            coreml,
            saved_model,
            pb,
            tflite,
            edgetpu,
            tfjs,
            paddle,
            mnn,
            ncnn,
            imx,
            rknn,
            pte,
            axelera,
            triton,
        ) = self._model_type("" if nn_module else model)
        fp16 &= pt or jit or onnx or xml or engine or nn_module or triton  # FP16
        nhwc = coreml or saved_model or pb or tflite or edgetpu or rknn  # BHWC formats (vs torch BCHW)
        stride, ch = 32, 3  # default stride and channels
        end2end, dynamic = False, False
        metadata, task = None, None

        # Set device
        cuda = isinstance(device, torch.device) and torch.cuda.is_available() and device.type != "cpu"  # use CUDA
        if cuda and not any([nn_module, pt, jit, engine, onnx, paddle]):  # GPU dataloader formats
            device = torch.device("cpu")
            cuda = False

        # Download if not local
        w = attempt_download_asset(model) if pt else model  # weights path

        # PyTorch (in-memory or file)
        if nn_module or pt:
            if nn_module:
                pt = True
                if fuse:
                    if IS_JETSON and is_jetson(jetpack=5):
                        # Jetson Jetpack5 requires device before fuse https://github.com/ultralytics/ultralytics/pull/21028
                        model = model.to(device)
                    model = model.fuse(verbose=verbose)
                model = model.to(device)
            else:  # pt file
                from ultralytics.nn.tasks import load_checkpoint

                model, _ = load_checkpoint(model, device=device, fuse=fuse)  # load model, ckpt

            # Common PyTorch model processing
            if hasattr(model, "kpt_shape"):
                kpt_shape = model.kpt_shape  # pose-only
            stride = max(int(model.stride.max()), 32)  # model stride
            names = model.module.names if hasattr(model, "module") else model.names  # get class names
            model.half() if fp16 else model.float()
            ch = model.yaml.get("channels", 3)
            for p in model.parameters():
                p.requires_grad = False
            self.model = model  # explicitly assign for to(), cpu(), cuda(), half()

        # TorchScript
        elif jit:
            import torchvision  # noqa - https://github.com/ultralytics/ultralytics/pull/19747

            LOGGER.info(f"Loading {w} for TorchScript inference...")
            extra_files = {"config.txt": ""}  # model metadata
            model = torch.jit.load(w, _extra_files=extra_files, map_location=device)
            model.half() if fp16 else model.float()
            if extra_files["config.txt"]:  # load metadata dict
                metadata = json.loads(extra_files["config.txt"], object_hook=lambda x: dict(x.items()))

        # ONNX OpenCV DNN
        elif dnn:
            LOGGER.info(f"Loading {w} for ONNX OpenCV DNN inference...")
            check_requirements("opencv-python>=4.5.4")
            net = cv2.dnn.readNetFromONNX(w)

        # ONNX Runtime and IMX
        elif onnx or imx:
            LOGGER.info(f"Loading {w} for ONNX Runtime inference...")
            check_requirements(("onnx", "onnxruntime-gpu" if cuda else "onnxruntime"))
            import onnxruntime

            # Select execution provider: CUDA > CoreML (mps) > CPU
            available = onnxruntime.get_available_providers()
            if cuda and "CUDAExecutionProvider" in available:
                providers = [("CUDAExecutionProvider", {"device_id": device.index}), "CPUExecutionProvider"]
            elif device.type == "mps" and "CoreMLExecutionProvider" in available:
                providers = ["CoreMLExecutionProvider", "CPUExecutionProvider"]
            else:
                providers = ["CPUExecutionProvider"]
                if cuda:
                    LOGGER.warning("CUDA requested but CUDAExecutionProvider not available. Using CPU...")
                    device, cuda = torch.device("cpu"), False
            LOGGER.info(
                f"Using ONNX Runtime {onnxruntime.__version__} with {providers[0] if isinstance(providers[0], str) else providers[0][0]}"
            )
            if onnx:
                session = onnxruntime.InferenceSession(w, providers=providers)
            else:
                check_requirements(("model-compression-toolkit>=2.4.1", "edge-mdt-cl<1.1.0", "onnxruntime-extensions"))
                w = next(Path(w).glob("*.onnx"))
                LOGGER.info(f"Loading {w} for ONNX IMX inference...")
                import mct_quantizers as mctq
                from edgemdt_cl.pytorch.nms import nms_ort  # noqa - register custom NMS ops

                session_options = mctq.get_ort_session_options()
                session_options.enable_mem_reuse = False  # fix the shape mismatch from onnxruntime
                session = onnxruntime.InferenceSession(w, session_options, providers=["CPUExecutionProvider"])

            output_names = [x.name for x in session.get_outputs()]
            metadata = session.get_modelmeta().custom_metadata_map
            dynamic = isinstance(session.get_outputs()[0].shape[0], str)
            fp16 = "float16" in session.get_inputs()[0].type

            # Setup IO binding for optimized inference (CUDA only, not supported for CoreML)
            use_io_binding = not dynamic and cuda
            if use_io_binding:
                io = session.io_binding()
                bindings = []
                for output in session.get_outputs():
                    out_fp16 = "float16" in output.type
                    y_tensor = torch.empty(output.shape, dtype=torch.float16 if out_fp16 else torch.float32).to(device)
                    io.bind_output(
                        name=output.name,
                        device_type=device.type,
                        device_id=device.index if cuda else 0,
                        element_type=np.float16 if out_fp16 else np.float32,
                        shape=tuple(y_tensor.shape),
                        buffer_ptr=y_tensor.data_ptr(),
                    )
                    bindings.append(y_tensor)

        # OpenVINO
        elif xml:
            LOGGER.info(f"Loading {w} for OpenVINO inference...")
            check_requirements("openvino>=2024.0.0")
            import openvino as ov

            core = ov.Core()
            device_name = "AUTO"
            if isinstance(device, str) and device.startswith("intel"):
                device_name = device.split(":")[1].upper()  # Intel OpenVINO device
                device = torch.device("cpu")
                if device_name not in core.available_devices:
                    LOGGER.warning(f"OpenVINO device '{device_name}' not available. Using 'AUTO' instead.")
                    device_name = "AUTO"
            w = Path(w)
            if not w.is_file():  # if not *.xml
                w = next(w.glob("*.xml"))  # get *.xml file from *_openvino_model dir
            ov_model = core.read_model(model=str(w), weights=w.with_suffix(".bin"))
            if ov_model.get_parameters()[0].get_layout().empty:
                ov_model.get_parameters()[0].set_layout(ov.Layout("NCHW"))

            metadata = w.parent / "metadata.yaml"
            if metadata.exists():
                metadata = YAML.load(metadata)
                batch = metadata["batch"]
                dynamic = metadata.get("args", {}).get("dynamic", dynamic)
            # OpenVINO inference modes are 'LATENCY', 'THROUGHPUT' (not recommended), or 'CUMULATIVE_THROUGHPUT'
            inference_mode = "CUMULATIVE_THROUGHPUT" if batch > 1 and dynamic else "LATENCY"
            ov_compiled_model = core.compile_model(
                ov_model,
                device_name=device_name,
                config={"PERFORMANCE_HINT": inference_mode},
            )
            LOGGER.info(
                f"Using OpenVINO {inference_mode} mode for batch={batch} inference on {', '.join(ov_compiled_model.get_property('EXECUTION_DEVICES'))}..."
            )
            input_name = ov_compiled_model.input().get_any_name()

        # TensorRT
        elif engine:
            LOGGER.info(f"Loading {w} for TensorRT inference...")

            if IS_JETSON and check_version(PYTHON_VERSION, "<=3.8.10"):
                # fix error: `np.bool` was a deprecated alias for the builtin `bool` for JetPack 4 and JetPack 5 with Python <= 3.8.10
                check_requirements("numpy==1.23.5")

            try:  # https://developer.nvidia.com/nvidia-tensorrt-download
                import tensorrt as trt
            except ImportError:
                if LINUX:
                    check_requirements("tensorrt>7.0.0,!=10.1.0")
                import tensorrt as trt
            check_version(trt.__version__, ">=7.0.0", hard=True)
            check_version(trt.__version__, "!=10.1.0", msg="https://github.com/ultralytics/ultralytics/pull/14239")
            if device.type == "cpu":
                device = torch.device("cuda:0")
            Binding = namedtuple("Binding", ("name", "dtype", "shape", "data", "ptr"))
            logger = trt.Logger(trt.Logger.INFO)
            # Read file
            with open(w, "rb") as f, trt.Runtime(logger) as runtime:
                try:
                    meta_len = int.from_bytes(f.read(4), byteorder="little")  # read metadata length
                    metadata = json.loads(f.read(meta_len).decode("utf-8"))  # read metadata
                    dla = metadata.get("dla", None)
                    if dla is not None:
                        runtime.DLA_core = int(dla)
                except UnicodeDecodeError:
                    f.seek(0)  # engine file may lack embedded Ultralytics metadata
                model = runtime.deserialize_cuda_engine(f.read())  # read engine

            # Model context
            try:
                context = model.create_execution_context()
            except Exception as e:  # model is None
                LOGGER.error(f"TensorRT model exported with a different version than {trt.__version__}\n")
                raise e

            bindings = OrderedDict()
            output_names = []
            fp16 = False  # default updated below
            dynamic = False
            is_trt10 = not hasattr(model, "num_bindings")
            num = range(model.num_io_tensors) if is_trt10 else range(model.num_bindings)
            for i in num:
                # Get tensor info using TRT10+ or legacy API
                if is_trt10:
                    name = model.get_tensor_name(i)
                    dtype = trt.nptype(model.get_tensor_dtype(name))
                    is_input = model.get_tensor_mode(name) == trt.TensorIOMode.INPUT
                    shape = tuple(model.get_tensor_shape(name))
                    profile_shape = tuple(model.get_tensor_profile_shape(name, 0)[2]) if is_input else None
                else:
                    name = model.get_binding_name(i)
                    dtype = trt.nptype(model.get_binding_dtype(i))
                    is_input = model.binding_is_input(i)
                    shape = tuple(model.get_binding_shape(i))
                    profile_shape = tuple(model.get_profile_shape(0, i)[1]) if is_input else None

                # Process input/output tensors
                if is_input:
                    if -1 in shape:
                        dynamic = True
                        if is_trt10:
                            context.set_input_shape(name, profile_shape)
                        else:
                            context.set_binding_shape(i, profile_shape)
                    if dtype == np.float16:
                        fp16 = True
                else:
                    output_names.append(name)
                shape = tuple(context.get_tensor_shape(name)) if is_trt10 else tuple(context.get_binding_shape(i))
                im = torch.from_numpy(np.empty(shape, dtype=dtype)).to(device)
                bindings[name] = Binding(name, dtype, shape, im, int(im.data_ptr()))
            binding_addrs = OrderedDict((n, d.ptr) for n, d in bindings.items())

        # CoreML
        elif coreml:
            check_requirements(
                ["coremltools>=9.0", "numpy>=1.14.5,<=2.3.5"]
            )  # latest numpy 2.4.0rc1 breaks coremltools exports
            LOGGER.info(f"Loading {w} for CoreML inference...")
            import coremltools as ct

            model = ct.models.MLModel(w)
            dynamic = model.get_spec().description.input[0].type.HasField("multiArrayType")
            metadata = dict(model.user_defined_metadata)

        # TF SavedModel
        elif saved_model:
            LOGGER.info(f"Loading {w} for TensorFlow SavedModel inference...")
            import tensorflow as tf

            model = tf.saved_model.load(w)
            metadata = Path(w) / "metadata.yaml"

        # TF GraphDef
        elif pb:  # https://www.tensorflow.org/guide/migrate#a_graphpb_or_graphpbtxt
            LOGGER.info(f"Loading {w} for TensorFlow GraphDef inference...")
            import tensorflow as tf

            from ultralytics.utils.export.tensorflow import gd_outputs

            def wrap_frozen_graph(gd, inputs, outputs):
                """Wrap frozen graphs for deployment."""
                x = tf.compat.v1.wrap_function(lambda: tf.compat.v1.import_graph_def(gd, name=""), [])  # wrapped
                ge = x.graph.as_graph_element
                return x.prune(tf.nest.map_structure(ge, inputs), tf.nest.map_structure(ge, outputs))

            gd = tf.Graph().as_graph_def()  # TF GraphDef
            with open(w, "rb") as f:
                gd.ParseFromString(f.read())
            frozen_func = wrap_frozen_graph(gd, inputs="x:0", outputs=gd_outputs(gd))
            try:  # find metadata in SavedModel alongside GraphDef
                metadata = next(Path(w).resolve().parent.rglob(f"{Path(w).stem}_saved_model*/metadata.yaml"))
            except StopIteration:
                pass

        # TFLite or TFLite Edge TPU
        elif tflite or edgetpu:  # https://ai.google.dev/edge/litert/microcontrollers/python
            try:  # https://coral.ai/docs/edgetpu/tflite-python/#update-existing-tf-lite-code-for-the-edge-tpu
                from tflite_runtime.interpreter import Interpreter, load_delegate
            except ImportError:
                import tensorflow as tf

                Interpreter, load_delegate = tf.lite.Interpreter, tf.lite.experimental.load_delegate
            if edgetpu:  # TF Edge TPU https://coral.ai/software/#edgetpu-runtime
                device = device[3:] if str(device).startswith("tpu") else ":0"
                LOGGER.info(f"Loading {w} on device {device[1:]} for TensorFlow Lite Edge TPU inference...")
                delegate = {"Linux": "libedgetpu.so.1", "Darwin": "libedgetpu.1.dylib", "Windows": "edgetpu.dll"}[
                    platform.system()
                ]
                interpreter = Interpreter(
                    model_path=w,
                    experimental_delegates=[load_delegate(delegate, options={"device": device})],
                )
                device = "cpu"  # Required, otherwise PyTorch will try to use the wrong device
            else:  # TFLite
                LOGGER.info(f"Loading {w} for TensorFlow Lite inference...")
                interpreter = Interpreter(model_path=w)  # load TFLite model
            interpreter.allocate_tensors()  # allocate
            input_details = interpreter.get_input_details()  # inputs
            output_details = interpreter.get_output_details()  # outputs
            # Load metadata
            try:
                with zipfile.ZipFile(w, "r") as zf:
                    name = zf.namelist()[0]
                    contents = zf.read(name).decode("utf-8")
                    if name == "metadata.json":  # Custom Ultralytics metadata dict for Python>=3.12
                        metadata = json.loads(contents)
                    else:
                        metadata = ast.literal_eval(contents)  # Default tflite-support metadata for Python<=3.11
            except (zipfile.BadZipFile, SyntaxError, ValueError, json.JSONDecodeError):
                pass

        # TF.js
        elif tfjs:
            raise NotImplementedError("Ultralytics TF.js inference is not currently supported.")

        # PaddlePaddle
        elif paddle:
            LOGGER.info(f"Loading {w} for PaddlePaddle inference...")
            check_requirements(
                "paddlepaddle-gpu"
                if torch.cuda.is_available()
                else "paddlepaddle==3.0.0"  # pin 3.0.0 for ARM64
                if ARM64
                else "paddlepaddle>=3.0.0"
            )
            import paddle.inference as pdi

            w = Path(w)
            model_file, params_file = None, None
            if w.is_dir():
                model_file = next(w.rglob("*.json"), None)
                params_file = next(w.rglob("*.pdiparams"), None)
            elif w.suffix == ".pdiparams":
                model_file = w.with_name("model.json")
                params_file = w

            if not (model_file and params_file and model_file.is_file() and params_file.is_file()):
                raise FileNotFoundError(f"Paddle model not found in {w}. Both .json and .pdiparams files are required.")

            config = pdi.Config(str(model_file), str(params_file))
            if cuda:
                config.enable_use_gpu(memory_pool_init_size_mb=2048, device_id=0)
            predictor = pdi.create_predictor(config)
            input_handle = predictor.get_input_handle(predictor.get_input_names()[0])
            output_names = predictor.get_output_names()
            metadata = w / "metadata.yaml"

        # MNN
        elif mnn:
            LOGGER.info(f"Loading {w} for MNN inference...")
            check_requirements("MNN")  # requires MNN
            import os

            import MNN

            config = {"precision": "low", "backend": "CPU", "numThread": (os.cpu_count() + 1) // 2}
            rt = MNN.nn.create_runtime_manager((config,))
            net = MNN.nn.load_module_from_file(w, [], [], runtime_manager=rt, rearrange=True)

            def torch_to_mnn(x):
                return MNN.expr.const(x.data_ptr(), x.shape)

            metadata = json.loads(net.get_info()["bizCode"])

        # NCNN
        elif ncnn:
            LOGGER.info(f"Loading {w} for NCNN inference...")
            check_requirements("git+https://github.com/Tencent/ncnn.git" if ARM64 else "ncnn", cmds="--no-deps")
            import ncnn as pyncnn

            net = pyncnn.Net()
            net.opt.use_vulkan_compute = cuda
            w = Path(w)
            if not w.is_file():  # if not *.param
                w = next(w.glob("*.param"))  # get *.param file from *_ncnn_model dir
            net.load_param(str(w))
            net.load_model(str(w.with_suffix(".bin")))
            metadata = w.parent / "metadata.yaml"

        # NVIDIA Triton Inference Server
        elif triton:
            check_requirements("tritonclient[all]")
            from ultralytics.utils.triton import TritonRemoteModel

            model = TritonRemoteModel(w)
            metadata = model.metadata

        # RKNN
        elif rknn:
            if not is_rockchip():
                raise OSError("RKNN inference is only supported on Rockchip devices.")
            LOGGER.info(f"Loading {w} for RKNN inference...")
            check_requirements("rknn-toolkit-lite2")
            from rknnlite.api import RKNNLite

            w = Path(w)
            if not w.is_file():  # if not *.rknn
                w = next(w.rglob("*.rknn"))  # get *.rknn file from *_rknn_model dir
            rknn_model = RKNNLite()
            rknn_model.load_rknn(str(w))
            rknn_model.init_runtime()
            metadata = w.parent / "metadata.yaml"

        # Axelera
        elif axelera:
            import os

            if not os.environ.get("AXELERA_RUNTIME_DIR"):
                LOGGER.warning(
                    "Axelera runtime environment is not activated."
                    "\nPlease run: source /opt/axelera/sdk/latest/axelera_activate.sh"
                    "\n\nIf this fails, verify driver installation: https://docs.ultralytics.com/integrations/axelera/#axelera-driver-installation"
                )
            try:
                from axelera.runtime import op
            except ImportError:
                check_requirements(
                    "axelera_runtime2==0.1.2",
                    cmds="--extra-index-url https://software.axelera.ai/artifactory/axelera-runtime-pypi",
                )
            from axelera.runtime import op

            w = Path(w)
            if (found := next(w.rglob("*.axm"), None)) is None:
                raise FileNotFoundError(f"No .axm file found in: {w}")

            ax_model = op.load(str(found))
            metadata = found.parent / "metadata.yaml"

        # ExecuTorch
        elif pte:
            LOGGER.info(f"Loading {w} for ExecuTorch inference...")
            # TorchAO release compatibility table bug https://github.com/pytorch/ao/issues/2919
            check_requirements("setuptools<71.0.0")  # Setuptools bug: https://github.com/pypa/setuptools/issues/4483
            check_requirements(("executorch==1.0.1", "flatbuffers"))
            from executorch.runtime import Runtime

            w = Path(w)
            if w.is_dir():
                model_file = next(w.rglob("*.pte"))
                metadata = w / "metadata.yaml"
            else:
                model_file = w
                metadata = w.parent / "metadata.yaml"

            program = Runtime.get().load_program(str(model_file))
            model = program.load_method("forward")

        # Any other format (unsupported)
        else:
            from ultralytics.engine.exporter import export_formats

            raise TypeError(
                f"model='{w}' is not a supported model format. Ultralytics supports: {export_formats()['Format']}\n"
                f"See https://docs.ultralytics.com/modes/predict for help."
            )

        # Load external metadata YAML
        if isinstance(metadata, (str, Path)) and Path(metadata).exists():
            metadata = YAML.load(metadata)
        if metadata and isinstance(metadata, dict):
            for k, v in metadata.items():
                if k in {"stride", "batch", "channels"}:
                    metadata[k] = int(v)
                elif k in {"imgsz", "names", "kpt_shape", "kpt_names", "args"} and isinstance(v, str):
                    metadata[k] = ast.literal_eval(v)
            stride = metadata["stride"]
            task = metadata["task"]
            batch = metadata["batch"]
            imgsz = metadata["imgsz"]
            names = metadata["names"]
            kpt_shape = metadata.get("kpt_shape")
            kpt_names = metadata.get("kpt_names")
            end2end = metadata.get("args", {}).get("nms", False)
            dynamic = metadata.get("args", {}).get("dynamic", dynamic)
            ch = metadata.get("channels", 3)
        elif not (pt or triton or nn_module):
            LOGGER.warning(f"Metadata not found for 'model={w}'")

        # Check names
        if "names" not in locals():  # names missing
            names = default_class_names(data)
        names = check_class_names(names)

        self.__dict__.update(locals())  # assign all variables to self
```

---
