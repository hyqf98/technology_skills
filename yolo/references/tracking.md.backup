# Yolo - Tracking

**Pages:** 12

---

## 使用Ultralytics YOLO11进行距离计算 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/distance-calculation/

**Contents:**
- 使用 Ultralytics YOLO11 进行距离计算
- 什么是距离计算？
- 视觉效果
- 距离计算的优势
  - DistanceCalculation() 参数
- 实施细节
- 应用
- 常见问题
  - 如何使用 Ultralytics YOLO11 计算物体之间的距离？
  - 将距离计算与 Ultralytics YOLO11 结合使用有哪些优势？

测量两个物体之间的间隙被称为指定空间内的距离计算。在 Ultralytics YOLO11 的情况下，边界框 质心用于计算用户突出显示的边界框的距离。

观看： 如何使用 Ultralytics YOLO 以像素为单位估计检测到的目标之间的距离 🚀

距离是一个估计值，可能不完全准确，因为它使用缺乏深度信息的2D数据进行计算。

使用Ultralytics YOLO进行距离计算

这是一个包含以下内容的表格 DistanceCalculation 参数：

您还可以使用各种 track 中的参数 DistanceCalculation 解决方案。

字段 DistanceCalculation 类的工作原理是跟踪视频帧中的对象，并计算所选边界框的质心之间的欧几里德距离。当您单击两个对象时，解决方案：

该实现使用 mouse_event_for_distance 方法来处理鼠标交互，允许用户根据需要选择对象和清除选择。该 process 方法处理逐帧处理、跟踪对象和计算距离。

使用YOLO11进行距离计算具有许多实际应用：

要使用以下方法计算对象之间的距离 Ultralytics YOLO11，您需要识别检测到的物体的边界框中心点。此过程涉及初始化 DistanceCalculation 类，来自 Ultralytics 的 solutions 模块，并使用模型的跟踪输出来计算距离。

将距离计算与 Ultralytics YOLO11 结合使用具有以下几个优点：

是的，您可以使用 Ultralytics YOLO11 在实时视频流中执行距离计算。该过程包括使用 OpenCV 捕获视频帧 OpenCV，运行 YOLO11 对象检测，并使用 DistanceCalculation 类，用于计算连续帧中对象之间的距离。有关详细的实现，请参见 视频流示例.

要删除在使用 Ultralytics YOLO11 进行距离计算期间绘制的点，您可以使用鼠标右键单击。此操作将清除您绘制的所有点。有关更多详细信息，请参阅 距离计算示例下的注释部分。

用于初始化以下对象的关键参数 DistanceCalculation Ultralytics YOLO11 中的类包括：

有关详尽的列表和默认值，请参阅DistanceCalculation 的参数。

**Examples:**

Example 1 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("distance_output.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Initialize distance calculation object
distancecalculator = solutions.DistanceCalculation(
    model="yolo11n.pt",  # path to the YOLO11 model file.
    show=True,  # display the output
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("Video frame is empty or processing is complete.")
        break

    results = distancecalculator(im0)

    print(results)  # access the output

    video_writer.write(results.plot_im)  # write the processed frame.

cap.release()
video_writer.release()
cv2.destroyAllWindows()  # destroy all opened windows
```

---

## Reference for ultralytics/solutions/trackzone.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/trackzone/

**Contents:**
- Reference for ultralytics/solutions/trackzone.py
- class ultralytics.solutions.trackzone.TrackZone
  - method ultralytics.solutions.trackzone.TrackZone.process

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/trackzone.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class to manage region-based object tracking in a video stream.

This class extends the BaseSolution class and provides functionality for tracking objects within a specific region defined by a polygonal area. Objects outside the region are excluded from tracking.

Process the input frame to track objects within a defined region.

This method initializes the annotator, creates a mask for the specified region, extracts tracks only from the masked area, and updates tracking information. Objects outside the region are ignored.

**Examples:**

Example 1 (rust):
```rust
TrackZone(self, **kwargs: Any) -> None
```

Example 2 (unknown):
```unknown
>>> tracker = TrackZone()
>>> frame = cv2.imread("frame.jpg")
>>> results = tracker.process(frame)
>>> cv2.imshow("Tracked Frame", results.plot_im)
```

Example 3 (python):
```python
class TrackZone(BaseSolution):
    """A class to manage region-based object tracking in a video stream.

    This class extends the BaseSolution class and provides functionality for tracking objects within a specific region
    defined by a polygonal area. Objects outside the region are excluded from tracking.

    Attributes:
        region (np.ndarray): The polygonal region for tracking, represented as a convex hull of points.
        line_width (int): Width of the lines used for drawing bounding boxes and region boundaries.
        names (list[str]): List of class names that the model can detect.
        boxes (list[np.ndarray]): Bounding boxes of tracked objects.
        track_ids (list[int]): Unique identifiers for each tracked object.
        clss (list[int]): Class indices of tracked objects.

    Methods:
        process: Process each frame of the video, applying region-based tracking.
        extract_tracks: Extract tracking information from the input frame.
        display_output: Display the processed output.

    Examples:
        >>> tracker = TrackZone()
        >>> frame = cv2.imread("frame.jpg")
        >>> results = tracker.process(frame)
        >>> cv2.imshow("Tracked Frame", results.plot_im)
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the TrackZone class for tracking objects within a defined region in video streams.

        Args:
            **kwargs (Any): Additional keyword arguments passed to the parent class.
        """
        super().__init__(**kwargs)
        default_region = [(75, 75), (565, 75), (565, 285), (75, 285)]
        self.region = cv2.convexHull(np.array(self.region or default_region, dtype=np.int32))
        self.mask = None
```

Example 4 (python):
```python
def process(self, im0: np.ndarray) -> SolutionResults
```

---

## Reference for ultralytics/trackers/utils/kalman_filter.py

**URL:** https://docs.ultralytics.com/zh/reference/trackers/utils/kalman_filter/

**Contents:**
- Reference for ultralytics/trackers/utils/kalman_filter.py
- class ultralytics.trackers.utils.kalman_filter.KalmanFilterXYAH
  - method ultralytics.trackers.utils.kalman_filter.KalmanFilterXYAH.gating_distance
  - method ultralytics.trackers.utils.kalman_filter.KalmanFilterXYAH.initiate
  - method ultralytics.trackers.utils.kalman_filter.KalmanFilterXYAH.multi_predict
  - method ultralytics.trackers.utils.kalman_filter.KalmanFilterXYAH.predict
  - method ultralytics.trackers.utils.kalman_filter.KalmanFilterXYAH.project
  - method ultralytics.trackers.utils.kalman_filter.KalmanFilterXYAH.update
- class ultralytics.trackers.utils.kalman_filter.KalmanFilterXYWH
  - method ultralytics.trackers.utils.kalman_filter.KalmanFilterXYWH.initiate

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/trackers/utils/kalman_filter.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A KalmanFilterXYAH class for tracking bounding boxes in image space using a Kalman filter.

Implements a simple Kalman filter for tracking bounding boxes in image space. The 8-dimensional state space (x, y, a, h, vx, vy, va, vh) contains the bounding box center position (x, y), aspect ratio a, height h, and their respective velocities. Object motion follows a constant velocity model, and bounding box location (x, y, a, h) is taken as a direct observation of the state space (linear observation model).

The Kalman filter is initialized with an 8-dimensional state space (x, y, a, h, vx, vy, va, vh), where (x, y) represents the bounding box center position, 'a' is the aspect ratio, 'h' is the height, and their respective velocities are (vx, vy, va, vh). The filter uses a constant velocity model for object motion and a linear observation model for bounding box location.

Compute gating distance between state distribution and measurements.

A suitable distance threshold can be obtained from chi2inv95. If only_position is False, the chi-square distribution has 4 degrees of freedom, otherwise 2.

Create a track from an unassociated measurement.

Run Kalman filter prediction step for multiple object states (Vectorized version).

Run Kalman filter prediction step.

Project state distribution to measurement space.

Run Kalman filter correction step.

Bases: KalmanFilterXYAH

A KalmanFilterXYWH class for tracking bounding boxes in image space using a Kalman filter.

Implements a Kalman filter for tracking bounding boxes with state space (x, y, w, h, vx, vy, vw, vh), where (x, y) is the center position, w is the width, h is the height, and vx, vy, vw, vh are their respective velocities. The object motion follows a constant velocity model, and the bounding box location (x, y, w, h) is taken as a direct observation of the state space (linear observation model).

Create track from unassociated measurement.

Run Kalman filter prediction step (Vectorized version).

Run Kalman filter prediction step.

Project state distribution to measurement space.

Run Kalman filter correction step.

**Examples:**

Example 1 (unknown):
```unknown
KalmanFilterXYAH(self)
```

Example 2 (sql):
```sql
Initialize the Kalman filter and create a track from a measurement
>>> kf = KalmanFilterXYAH()
>>> measurement = np.array([100, 200, 1.5, 50])
>>> mean, covariance = kf.initiate(measurement)
>>> print(mean)
>>> print(covariance)
```

Example 3 (python):
```python
class KalmanFilterXYAH:
    """A KalmanFilterXYAH class for tracking bounding boxes in image space using a Kalman filter.

    Implements a simple Kalman filter for tracking bounding boxes in image space. The 8-dimensional state space (x, y,
    a, h, vx, vy, va, vh) contains the bounding box center position (x, y), aspect ratio a, height h, and their
    respective velocities. Object motion follows a constant velocity model, and bounding box location (x, y, a, h) is
    taken as a direct observation of the state space (linear observation model).

    Attributes:
        _motion_mat (np.ndarray): The motion matrix for the Kalman filter.
        _update_mat (np.ndarray): The update matrix for the Kalman filter.
        _std_weight_position (float): Standard deviation weight for position.
        _std_weight_velocity (float): Standard deviation weight for velocity.

    Methods:
        initiate: Create a track from an unassociated measurement.
        predict: Run the Kalman filter prediction step.
        project: Project the state distribution to measurement space.
        multi_predict: Run the Kalman filter prediction step (vectorized version).
        update: Run the Kalman filter correction step.
        gating_distance: Compute the gating distance between state distribution and measurements.

    Examples:
        Initialize the Kalman filter and create a track from a measurement
        >>> kf = KalmanFilterXYAH()
        >>> measurement = np.array([100, 200, 1.5, 50])
        >>> mean, covariance = kf.initiate(measurement)
        >>> print(mean)
        >>> print(covariance)
    """

    def __init__(self):
        """Initialize Kalman filter model matrices with motion and observation uncertainty weights.

        The Kalman filter is initialized with an 8-dimensional state space (x, y, a, h, vx, vy, va, vh), where (x, y)
        represents the bounding box center position, 'a' is the aspect ratio, 'h' is the height, and their respective
        velocities are (vx, vy, va, vh). The filter uses a constant velocity model for object motion and a linear
        observation model for bounding box location.
        """
        ndim, dt = 4, 1.0

        # Create Kalman filter model matrices
        self._motion_mat = np.eye(2 * ndim, 2 * ndim)
        for i in range(ndim):
            self._motion_mat[i, ndim + i] = dt
        self._update_mat = np.eye(ndim, 2 * ndim)

        # Motion and observation uncertainty are chosen relative to the current state estimate
        self._std_weight_position = 1.0 / 20
        self._std_weight_velocity = 1.0 / 160
```

Example 4 (typescript):
```typescript
def gating_distance(
    self,
    mean: np.ndarray,
    covariance: np.ndarray,
    measurements: np.ndarray,
    only_position: bool = False,
    metric: str = "maha",
) -> np.ndarray
```

---

## 使用 Ultralytics YOLO11 的 VisionEye 视图对象映射 🚀

**URL:** https://docs.ultralytics.com/zh/guides/vision-eye/

**Contents:**
- 使用 Ultralytics YOLO11 的 VisionEye 视图对象映射 🚀
- 什么是 VisionEye Object Mapping？
  - VisionEye 参数
- VisionEye 的工作原理
- VisionEye 的应用
- 注意
- 常见问题
  - 如何开始将 VisionEye Object Mapping 与 Ultralytics YOLO11 结合使用？
  - 为什么我应该使用 Ultralytics YOLO11 进行对象映射和跟踪？
  - 如何将 VisionEye 与其他 机器学习 工具（如 Comet 或 ClearML）集成？

Ultralytics YOLO11 VisionEye 使计算机能够识别和精确定位对象，模拟人眼的观察精度。此功能使计算机能够辨别和专注于特定对象，就像人眼从特定视点观察细节的方式一样。

使用 Ultralytics YOLO 进行 VisionEye 映射

字段 vision_point 元组表示观察者在像素坐标中的位置。调整它以匹配相机视角，从而使渲染的光线正确地说明物体与所选视点的关系。

这是一个包含以下内容的表格 VisionEye 参数：

您还可以使用各种 track 内的参数 VisionEye 解决方案：

VisionEye 的工作原理是在帧中建立一个固定的视觉点，并从该点绘制到检测到的对象。这模拟了人类视觉如何从单个视点关注多个对象。该解决方案使用对象跟踪来保持跨帧对象识别的一致性，从而创建观察者（视觉点）和场景中对象之间空间关系的可视化表示。

字段 process VisionEye 类中的 方法执行几个关键操作：

这种方法对于需要空间感知和对象关系可视化的应用（如监控系统、自主导航和交互式装置）特别有用。

VisionEye 对象映射在各个行业中具有许多实际应用：

通过将 VisionEye 与其他 Ultralytics 解决方案（如 距离计算 或 速度估算）相结合，您可以构建全面的系统，这些系统不仅可以跟踪物体，还可以理解它们的空间关系和行为。

如有任何疑问，请随时在 Ultralytics Issue Section 或下面提到的讨论区中发布您的问题。

要开始将 VisionEye 对象映射与 Ultralytics YOLO11 结合使用，首先需要通过 pip 安装 Ultralytics YOLO 包。然后，您可以使用文档中提供的示例代码来设置 对象检测 与 VisionEye 的结合使用。以下是一个简单的入门示例：

Ultralytics YOLO11 以其速度、准确性和易于集成而闻名，使其成为对象映射和跟踪的首选。主要优势包括：

有关应用和优势的更多信息，请查看 Ultralytics YOLO11 文档。

Ultralytics YOLO11 可以与各种机器学习工具（如 Comet 和 ClearML）无缝集成，从而增强实验跟踪、协作和可重复性。请按照关于如何将 YOLOv5 与 Comet 结合使用以及将 YOLO11 与 ClearML 集成的详细指南开始使用。

如需进一步探索和集成示例，请查看我们的Ultralytics 集成指南。

**Examples:**

Example 1 (markdown):
```markdown
# Monitor objects position with visioneye
yolo solutions visioneye show=True

# Pass a source video
yolo solutions visioneye source="path/to/video.mp4"

# Monitor the specific classes
yolo solutions visioneye classes="[0, 5]"
```

Example 2 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("visioneye_output.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Initialize vision eye object
visioneye = solutions.VisionEye(
    show=True,  # display the output
    model="yolo11n.pt",  # use any model that Ultralytics supports, e.g., YOLOv10
    classes=[0, 2],  # generate visioneye view for specific classes
    vision_point=(50, 50),  # the point where VisionEye will view objects and draw tracks
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("Video frame is empty or video processing has been successfully completed.")
        break

    results = visioneye(im0)

    print(results)  # access the output

    video_writer.write(results.plot_im)  # write the video file

cap.release()
video_writer.release()
cv2.destroyAllWindows()  # destroy all opened windows
```

Example 3 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("vision-eye-mapping.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Init vision eye object
visioneye = solutions.VisionEye(
    show=True,  # display the output
    model="yolo11n.pt",  # use any model that Ultralytics supports, e.g., YOLOv10
    classes=[0, 2],  # generate visioneye view for specific classes
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("Video frame is empty or video processing has been successfully completed.")
        break

    results = visioneye(im0)

    print(results)  # access the output

    video_writer.write(results.plot_im)  # write the video file

cap.release()
video_writer.release()
cv2.destroyAllWindows()  # destroy all opened windows
```

---

## Reference for hub_sdk/base/paginated_list.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/base/paginated_list/

**Contents:**
- Reference for hub_sdk/base/paginated_list.py
- class hub_sdk.base.paginated_list.PaginatedList
  - method hub_sdk.base.paginated_list.PaginatedList.__update_data
  - method hub_sdk.base.paginated_list.PaginatedList._get
  - method hub_sdk.base.paginated_list.PaginatedList.list
  - method hub_sdk.base.paginated_list.PaginatedList.next
  - method hub_sdk.base.paginated_list.PaginatedList.previous

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/base/paginated_list.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Handles pagination for list endpoints on the API while managing retrieval, navigation, and updating of data.

This class extends APIClient to provide pagination functionality for API endpoints that return large datasets. It manages page navigation, data retrieval, and state tracking across paginated results.

Update the internal data with the response from the API.

Retrieve data for the current page.

Retrieve a list of items from the API.

Move to the next page of results if available.

Move to the previous page of results if available.

**Examples:**

Example 1 (rust):
```rust
PaginatedList(self, base_endpoint, name, page_size = None, public = None, headers = None)
```

Example 2 (python):
```python
class PaginatedList(APIClient):
    """Handles pagination for list endpoints on the API while managing retrieval, navigation, and updating of data.

    This class extends APIClient to provide pagination functionality for API endpoints that return large datasets. It
    manages page navigation, data retrieval, and state tracking across paginated results.

    Attributes:
        name (str): Descriptive name for the paginated resource.
        page_size (int): Number of items to display per page.
        public (bool, optional): Filter for public resources if specified.
        pages (List): List tracking page identifiers for navigation.
        current_page (int): Index of the currently displayed page.
        total_pages (int): Total number of available pages.
        results (dict): Current page results from the API.

    Methods:
        previous: Navigate to the previous page of results.
        next: Navigate to the next page of results.
        list: Retrieve a list of items from the API with pagination parameters.
    """

    def __init__(self, base_endpoint, name, page_size=None, public=None, headers=None):
        """Initialize a PaginatedList instance.

        Args:
            base_endpoint (str): The base API endpoint for the paginated resource.
            name (str): A descriptive name for the paginated resource.
            page_size (int, optional): The number of items per page.
            public (bool, optional): Filter for public resources if specified.
            headers (dict, optional): Additional headers to include in API requests.
        """
        super().__init__(f"{HUB_FUNCTIONS_ROOT}/v1/{base_endpoint}", headers)
        self.name = name
        self.page_size = page_size
        self.public = public
        self.pages = [None]
        self.current_page = 0
        self.total_pages = 1
        self._get()
```

Example 3 (python):
```python
def __update_data(self, resp: Response) -> None
```

Example 4 (python):
```python
def __update_data(self, resp: Response) -> None:
    """Update the internal data with the response from the API.

    Args:
        resp (Response): API response data containing pagination information and results.
    """
    if resp:
        resp_data = resp.json().get("data", {})
        self.results = resp_data.get("results", {})
        self.total_pages = math.ceil(resp_data.get("total") / self.page_size) if self.page_size > 0 else 0
        last_record_id = resp_data.get("lastRecordId")
        if last_record_id is None:
            self.pages[self.current_page + 1 :] = [None] * (len(self.pages) - self.current_page - 1)
        elif len(self.pages) <= self.current_page + 1:
            self.pages.append(last_record_id)
        else:
            self.pages[self.current_page + 1] = last_record_id
    else:
        self.results = {}
        self.total_pages = 0
        self.pages[self.current_page + 1 :] = [None] * (len(self.pages) - self.current_page - 1)
```

---

## Reference for ultralytics/solutions/queue_management.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/queue_management/

**Contents:**
- Reference for ultralytics/solutions/queue_management.py
- class ultralytics.solutions.queue_management.QueueManager
  - method ultralytics.solutions.queue_management.QueueManager.process

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/queue_management.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Manages queue counting in real-time video streams based on object tracks.

This class extends BaseSolution to provide functionality for tracking and counting objects within a specified region in video frames.

Process queue management for a single frame of video.

**Examples:**

Example 1 (rust):
```rust
QueueManager(self, **kwargs: Any) -> None
```

Example 2 (unknown):
```unknown
>>> cap = cv2.VideoCapture("path/to/video.mp4")
>>> queue_manager = QueueManager(region=[100, 100, 200, 200, 300, 300])
>>> while cap.isOpened():
...     success, im0 = cap.read()
...     if not success:
...         break
...     results = queue_manager.process(im0)
```

Example 3 (python):
```python
class QueueManager(BaseSolution):
    """Manages queue counting in real-time video streams based on object tracks.

    This class extends BaseSolution to provide functionality for tracking and counting objects within a specified region
    in video frames.

    Attributes:
        counts (int): The current count of objects in the queue.
        rect_color (tuple[int, int, int]): BGR color tuple for drawing the queue region rectangle.
        region_length (int): The number of points defining the queue region.
        track_line (list[tuple[int, int]]): List of track line coordinates.
        track_history (dict[int, list[tuple[int, int]]]): Dictionary storing tracking history for each object.

    Methods:
        initialize_region: Initialize the queue region.
        process: Process a single frame for queue management.
        extract_tracks: Extract object tracks from the current frame.
        store_tracking_history: Store the tracking history for an object.
        display_output: Display the processed output.

    Examples:
        >>> cap = cv2.VideoCapture("path/to/video.mp4")
        >>> queue_manager = QueueManager(region=[100, 100, 200, 200, 300, 300])
        >>> while cap.isOpened():
        ...     success, im0 = cap.read()
        ...     if not success:
        ...         break
        ...     results = queue_manager.process(im0)
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the QueueManager with parameters for tracking and counting objects in a video stream."""
        super().__init__(**kwargs)
        self.initialize_region()
        self.counts = 0  # Queue counts information
        self.rect_color = (255, 255, 255)  # Rectangle color for visualization
        self.region_length = len(self.region)  # Store region length for further usage
```

Example 4 (python):
```python
def process(self, im0) -> SolutionResults
```

---

## 使用 Ultralytics YOLO11 进行锻炼监控 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/workouts-monitoring/

**Contents:**
- 使用 Ultralytics YOLO11 进行锻炼监控
- 锻炼监控的优势
- 实际应用
  - 关键点图
  - AIGym 参数
- 常见问题
  - 如何使用 Ultralytics YOLO11 监控我的训练过程？
  - 使用 Ultralytics YOLO11 进行健身监控有哪些好处？
  - Ultralytics YOLO11 在检测和跟踪运动方面的准确性如何？
  - 我可以将Ultralytics YOLO11用于自定义锻炼程序吗？

通过 Ultralytics YOLO11 的姿势估计来监控锻炼，可以通过实时准确地跟踪关键身体地标和关节来增强运动评估。这项技术可以提供关于运动姿势的即时反馈，跟踪锻炼程序，并测量性能指标，从而为用户和教练优化训练课程。

观看： 如何使用 Ultralytics YOLO 监控锻炼动作 | 深蹲、腿部伸展、俯卧撑等

使用 Ultralytics YOLO 进行锻炼监控

这是一个包含以下内容的表格 AIGym 参数：

字段 AIGym 解决方案还支持一系列目标跟踪参数：

要使用 Ultralytics YOLO11 监控您的锻炼，您可以利用姿势估计功能实时 track 和分析关键身体地标和关节。这使您能够即时获得有关锻炼姿势的反馈、计算重复次数并衡量性能指标。您可以从使用提供的俯卧撑、引体向上或腹肌锻炼示例代码开始，如下所示：

有关更多自定义和设置，您可以参考文档中的AIGym部分。

使用 Ultralytics YOLO11 进行健身监控可提供多个关键优势：

您可以观看 YouTube 视频演示，了解这些优势的实际应用。

Ultralytics YOLO11 在检测和跟踪运动方面具有极高精度，这得益于其先进的姿势估计能力。它能准确跟踪关键身体地标和关节，提供关于运动姿态和性能指标的实时反馈。该模型的预训练权重和稳健架构确保了高精度和可靠性。有关实际应用示例，请查阅文档中的实际应用部分，其中展示了俯卧撑和引体向上的计数功能。

是的，Ultralytics YOLO11 可以适用于自定义的锻炼程序。该 AIGym class 支持不同的姿势类型，例如 pushup, pullup和 abworkout。您可以指定关键点和角度来detect特定的练习。这是一个示例设置：

有关设置参数的更多详细信息，请参阅 参数 AIGym 部分。这种灵活性使您可以监控各种练习，并根据您的情况自定义例程 健身目标.

要保存锻炼监控输出，您可以修改代码以包含一个视频写入器，用于保存处理后的帧。以下是一个示例：

此设置将监控的视频写入输出文件，使您可以稍后查看您的锻炼表现或与教练分享，以获得更多反馈。

**Examples:**

Example 1 (markdown):
```markdown
# Run a workout example
yolo solutions workout show=True

# Pass a source video
yolo solutions workout source="path/to/video.mp4"

# Use keypoints for pushups
yolo solutions workout kpts="[6, 8, 10]"
```

Example 2 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("workouts_output.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Init AIGym
gym = solutions.AIGym(
    show=True,  # display the frame
    kpts=[6, 8, 10],  # keypoints for monitoring specific exercise, by default it's for pushup
    model="yolo11n-pose.pt",  # path to the YOLO11 pose estimation model file
    # line_width=2,  # adjust the line width for bounding boxes and text display
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("Video frame is empty or processing is complete.")
        break

    results = gym(im0)

    # print(results)  # access the output

    video_writer.write(results.plot_im)  # write the processed frame.

cap.release()
video_writer.release()
cv2.destroyAllWindows()  # destroy all opened windows
```

Example 3 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))

gym = solutions.AIGym(
    line_width=2,
    show=True,
    kpts=[6, 8, 10],
)

while cap.isOpened():
    success, im0 = cap.read()
    if not success:
        print("Video frame is empty or processing is complete.")
        break
    results = gym(im0)

cv2.destroyAllWindows()
```

Example 4 (python):
```python
from ultralytics import solutions

gym = solutions.AIGym(
    line_width=2,
    show=True,
    kpts=[6, 8, 10],  # For pushups - can be customized for other exercises
)
```

---

## Reference for ultralytics/solutions/speed_estimation.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/speed_estimation/

**Contents:**
- Reference for ultralytics/solutions/speed_estimation.py
- class ultralytics.solutions.speed_estimation.SpeedEstimator
  - method ultralytics.solutions.speed_estimation.SpeedEstimator.process

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/speed_estimation.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class to estimate the speed of objects in a real-time video stream based on their tracks.

This class extends the BaseSolution class and provides functionality for estimating object speeds using tracking data in video streams. Speed is calculated based on pixel displacement over time and converted to real-world units using a configurable meters-per-pixel scale factor.

Process an input frame to estimate object speeds based on tracking data.

**Examples:**

Example 1 (rust):
```rust
SpeedEstimator(self, **kwargs: Any) -> None
```

Example 2 (unknown):
```unknown
Initialize speed estimator and process a frame
>>> estimator = SpeedEstimator(meter_per_pixel=0.04, max_speed=120)
>>> frame = cv2.imread("frame.jpg")
>>> results = estimator.process(frame)
>>> cv2.imshow("Speed Estimation", results.plot_im)
```

Example 3 (python):
```python
class SpeedEstimator(BaseSolution):
    """A class to estimate the speed of objects in a real-time video stream based on their tracks.

    This class extends the BaseSolution class and provides functionality for estimating object speeds using tracking
    data in video streams. Speed is calculated based on pixel displacement over time and converted to real-world units
    using a configurable meters-per-pixel scale factor.

    Attributes:
        fps (float): Video frame rate for time calculations.
        frame_count (int): Global frame counter for tracking temporal information.
        trk_frame_ids (dict): Maps track IDs to their first frame index.
        spd (dict): Final speed per object in km/h once locked.
        trk_hist (dict): Maps track IDs to deque of position history.
        locked_ids (set): Track IDs whose speed has been finalized.
        max_hist (int): Required frame history before computing speed.
        meter_per_pixel (float): Real-world meters represented by one pixel for scene scale conversion.
        max_speed (int): Maximum allowed object speed; values above this will be capped.

    Methods:
        process: Process input frames to estimate object speeds based on tracking data.
        store_tracking_history: Store the tracking history for an object.
        extract_tracks: Extract tracks from the current frame.
        display_output: Display the output with annotations.

    Examples:
        Initialize speed estimator and process a frame
        >>> estimator = SpeedEstimator(meter_per_pixel=0.04, max_speed=120)
        >>> frame = cv2.imread("frame.jpg")
        >>> results = estimator.process(frame)
        >>> cv2.imshow("Speed Estimation", results.plot_im)
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the SpeedEstimator object with speed estimation parameters and data structures.

        Args:
            **kwargs (Any): Additional keyword arguments passed to the parent class.
        """
        super().__init__(**kwargs)

        self.fps = self.CFG["fps"]  # Video frame rate for time calculations
        self.frame_count = 0  # Global frame counter
        self.trk_frame_ids = {}  # Track ID → first frame index
        self.spd = {}  # Final speed per object (km/h), once locked
        self.trk_hist = {}  # Track ID → deque of (time, position)
        self.locked_ids = set()  # Track IDs whose speed has been finalized
        self.max_hist = self.CFG["max_hist"]  # Required frame history before computing speed
        self.meter_per_pixel = self.CFG["meter_per_pixel"]  # Scene scale, depends on camera details
        self.max_speed = self.CFG["max_speed"]  # Maximum speed adjustment
```

Example 4 (python):
```python
def process(self, im0) -> SolutionResults
```

---

## Reference for ultralytics/solutions/solutions.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/solutions/

**Contents:**
- Reference for ultralytics/solutions/solutions.py
- class ultralytics.solutions.solutions.BaseSolution
  - method ultralytics.solutions.solutions.BaseSolution.__call__
  - method ultralytics.solutions.solutions.BaseSolution.adjust_box_label
  - method ultralytics.solutions.solutions.BaseSolution.display_output
  - method ultralytics.solutions.solutions.BaseSolution.extract_tracks
  - method ultralytics.solutions.solutions.BaseSolution.initialize_region
  - method ultralytics.solutions.solutions.BaseSolution.process
  - method ultralytics.solutions.solutions.BaseSolution.store_tracking_history
- class ultralytics.solutions.solutions.SolutionAnnotator

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/solutions.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A base class for managing Ultralytics Solutions.

This class provides core functionality for various Ultralytics Solutions, including model loading, object tracking, and region initialization. It serves as the foundation for implementing specific computer vision solutions such as object counting, pose estimation, and analytics.

Allow instances to be called like a function with flexible arguments.

Generate a formatted label for a bounding box.

This method constructs a label string for a bounding box using the class index and confidence score. Optionally includes the track ID if provided. The label format adapts based on the display settings defined in self.show_conf and self.show_labels.

Display the results of the processing, which could involve showing frames, printing counts, or saving

This method is responsible for visualizing the output of the object detection and tracking process. It displays the processed frame with annotations, and allows for user interaction to close the display.

Apply object tracking and extract tracks from an input image or frame.

Initialize the counting region and line segment based on configuration settings.

Process method should be implemented by each Solution subclass.

Store the tracking history of an object.

This method updates the tracking history for a given object by appending the center point of its bounding box to the track line. It maintains a maximum of 30 points in the tracking history.

A specialized annotator class for visualizing and analyzing computer vision tasks.

This class extends the base Annotator class, providing additional methods for drawing regions, centroids, tracking trails, and visual annotations for Ultralytics Solutions. It offers comprehensive visualization capabilities for various computer vision applications including object detection, tracking, pose estimation, and analytics.

Calculate the angle between three points for workout monitoring (cached).

Convert a keypoint-like object to an (x, y) tuple of floats.

Draw a label with a background rectangle or circle centered within a given bounding box.

Display overall statistics for Solutions (e.g., parking management and object counting).

Display the bounding boxes labels in parking management app.

Draw a region or line on the image.

Draw specific keypoints for gym steps counting.

Keypoint format: [x, y] or [x, y, confidence]. Modifies self.im in-place.

Calculate the angle between three points for workout monitoring.

Plot the pose angle, count value, and step stage for workout monitoring.

Plot the distance and line between two centroids on the frame.

Draw workout text with a background on the image.

Display queue counts on an image centered at the points with customizable font size and colors.

Draw a sweep annotation line and an optional label.

Perform pinpoint human-vision eye mapping and plotting.

A class to encapsulate the results of Ultralytics Solutions.

This class is designed to store and manage various outputs generated by the solution pipeline, including counts, angles, workout stages, and other analytics data. It provides a structured way to access and manipulate results from different computer vision solutions such as object counting, pose estimation, and tracking analytics.

Return a formatted string representation of the SolutionResults object.

**Examples:**

Example 1 (typescript):
```typescript
BaseSolution(self, is_cli: bool = False, **kwargs: Any) -> None
```

Example 2 (unknown):
```unknown
>>> solution = BaseSolution(model="yolo11n.pt", region=[(0, 0), (100, 0), (100, 100), (0, 100)])
>>> solution.initialize_region()
>>> image = cv2.imread("image.jpg")
>>> solution.extract_tracks(image)
>>> solution.display_output(image)
```

Example 3 (python):
```python
class BaseSolution:
    """A base class for managing Ultralytics Solutions.

    This class provides core functionality for various Ultralytics Solutions, including model loading, object tracking,
    and region initialization. It serves as the foundation for implementing specific computer vision solutions such as
    object counting, pose estimation, and analytics.

    Attributes:
        LineString: Class for creating line string geometries from shapely.
        Polygon: Class for creating polygon geometries from shapely.
        Point: Class for creating point geometries from shapely.
        prep: Prepared geometry function from shapely for optimized spatial operations.
        CFG (dict[str, Any]): Configuration dictionary loaded from YAML file and updated with kwargs.
        LOGGER: Logger instance for solution-specific logging.
        annotator: Annotator instance for drawing on images.
        tracks: YOLO tracking results from the latest inference.
        track_data: Extracted tracking data (boxes or OBB) from tracks.
        boxes (list): Bounding box coordinates from tracking results.
        clss (list[int]): Class indices from tracking results.
        track_ids (list[int]): Track IDs from tracking results.
        confs (list[float]): Confidence scores from tracking results.
        track_line: Current track line for storing tracking history.
        masks: Segmentation masks from tracking results.
        r_s: Region or line geometry object for spatial operations.
        frame_no (int): Current frame number for logging purposes.
        region (list[tuple[int, int]]): List of coordinate tuples defining region of interest.
        line_width (int): Width of lines used in visualizations.
        model (YOLO): Loaded YOLO model instance.
        names (dict[int, str]): Dictionary mapping class indices to class names.
        classes (list[int]): List of class indices to track.
        show_conf (bool): Flag to show confidence scores in annotations.
        show_labels (bool): Flag to show class labels in annotations.
        device (str): Device for model inference.
        track_add_args (dict[str, Any]): Additional arguments for tracking configuration.
        env_check (bool): Flag indicating whether environment supports image display.
        track_history (defaultdict): Dictionary storing tracking history for each object.
        profilers (tuple): Profiler instances for performance monitoring.

    Methods:
        adjust_box_label: Generate formatted label for bounding box.
        extract_tracks: Apply object tracking and extract tracks from input image.
        store_tracking_history: Store object tracking history for given track ID and bounding box.
        initialize_region: Initialize counting region and line segment based on configuration.
        display_output: Display processing results including frames or saved results.
        process: Process method to be implemented by each Solution subclass.

    Examples:
        >>> solution = BaseSolution(model="yolo11n.pt", region=[(0, 0), (100, 0), (100, 100), (0, 100)])
        >>> solution.initialize_region()
        >>> image = cv2.imread("image.jpg")
        >>> solution.extract_tracks(image)
        >>> solution.display_output(image)
    """

    def __init__(self, is_cli: bool = False, **kwargs: Any) -> None:
        """Initialize the BaseSolution class with configuration settings and YOLO model.

        Args:
            is_cli (bool): Enable CLI mode if set to True.
            **kwargs (Any): Additional configuration parameters that override defaults.
        """
        self.CFG = vars(SolutionConfig().update(**kwargs))
        self.LOGGER = LOGGER  # Store logger object to be used in multiple solution classes

        check_requirements("shapely>=2.0.0")
        from shapely.geometry import LineString, Point, Polygon
        from shapely.prepared import prep

        self.LineString = LineString
        self.Polygon = Polygon
        self.Point = Point
        self.prep = prep
        self.annotator = None  # Initialize annotator
        self.tracks = None
        self.track_data = None
        self.boxes = []
        self.clss = []
        self.track_ids = []
        self.track_line = None
        self.masks = None
        self.r_s = None
        self.frame_no = -1  # Only for logging

        self.LOGGER.info(f"Ultralytics Solutions: ✅ {self.CFG}")
        self.region = self.CFG["region"]  # Store region data for other classes usage
        self.line_width = self.CFG["line_width"]

        # Load Model and store additional information (classes, show_conf, show_label)
        if self.CFG["model"] is None:
            self.CFG["model"] = "yolo11n.pt"
        self.model = YOLO(self.CFG["model"])
        self.names = self.model.names
        self.classes = self.CFG["classes"]
        self.show_conf = self.CFG["show_conf"]
        self.show_labels = self.CFG["show_labels"]
        self.device = self.CFG["device"]

        self.track_add_args = {  # Tracker additional arguments for advance configuration
            k: self.CFG[k] for k in {"iou", "conf", "device", "max_det", "half", "tracker"}
        }  # verbose must be passed to track method; setting it False in YOLO still logs the track information.

        if is_cli and self.CFG["source"] is None:
            d_s = "solutions_ci_demo.mp4" if "-pose" not in self.CFG["model"] else "solution_ci_pose_demo.mp4"
            self.LOGGER.warning(f"source not provided. using default source {ASSETS_URL}/{d_s}")
            from ultralytics.utils.downloads import safe_download

            safe_download(f"{ASSETS_URL}/{d_s}")  # download source from ultralytics assets
            self.CFG["source"] = d_s  # set default source

        # Initialize environment and region setup
        self.env_check = check_imshow(warn=True)
        self.track_history = defaultdict(list)

        self.profilers = (
            ops.Profile(device=self.device),  # track
            ops.Profile(device=self.device),  # solution
        )
```

Example 4 (python):
```python
def __call__(self, *args: Any, **kwargs: Any)
```

---

## 使用 Ultralytics YOLO11 的 TrackZone - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/trackzone/

**Contents:**
- 使用 Ultralytics YOLO11 的 TrackZone
- 什么是 TrackZone？
- 区域中对象跟踪的优势 (TrackZone)
- 实际应用
  - TrackZone 参数
- 常见问题
  - 如何使用 Ultralytics YOLO11 track 视频帧中特定区域或区域内的目标？
  - 如何在 Python 中将 TrackZone 与 Ultralytics YOLO11 结合使用？
  - 如何使用 Ultralytics TrackZone 配置视频处理的区域点？
- 评论

TrackZone 专门用于监视帧的指定区域内的对象，而不是整个帧。它基于 Ultralytics YOLO11 构建，集成了视频和实时摄像机馈送中特定区域内的对象检测和跟踪。YOLO11 的先进算法和 深度学习 技术使其成为实时用例的完美选择，可在人群监控和监视等应用中提供精确高效的对象跟踪。

观看： 如何使用 Ultralytics YOLO11 在区域中跟踪对象 | TrackZone 🚀

使用 Ultralytics YOLO 的 TrackZone

TrackZone 依赖于 region 列表，以了解要监控帧的哪个部分。定义多边形以匹配您关心的物理区域（门、大门等），并保持 show=True 在配置时启用，以便您可以验证叠加层是否与视频流对齐。

这是一个包含以下内容的表格 TrackZone 参数：

TrackZone 解决方案包括对以下内容的支持 track 参数：

使用 Ultralytics YOLO11 可以直接在视频帧的定义区域或区域内跟踪对象。只需使用下面提供的命令来启动跟踪。这种方法确保了高效的分析和准确的结果，使其成为监控、人群管理或任何需要区域跟踪的场景的理想选择。

只需几行代码，您就可以在特定区域设置目标跟踪，从而轻松集成到您的项目中。

使用 Ultralytics TrackZone 为视频处理配置区域点非常简单且可自定义。您可以通过 python 脚本直接定义和调整区域，从而精确控制要监控的区域。

**Examples:**

Example 1 (markdown):
```markdown
# Run a trackzone example
yolo solutions trackzone show=True

# Pass a source video
yolo solutions trackzone source="path/to/video.mp4" show=True

# Pass region coordinates
yolo solutions trackzone show=True region="[(150, 150), (1130, 150), (1130, 570), (150, 570)]"
```

Example 2 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Define region points
region_points = [(150, 150), (1130, 150), (1130, 570), (150, 570)]

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("trackzone_output.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Init trackzone (object tracking in zones, not complete frame)
trackzone = solutions.TrackZone(
    show=True,  # display the output
    region=region_points,  # pass region points
    model="yolo11n.pt",  # use any model that Ultralytics supports, e.g., YOLOv9, YOLOv10
    # line_width=2,  # adjust the line width for bounding boxes and text display
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()
    if not success:
        print("Video frame is empty or processing is complete.")
        break

    results = trackzone(im0)

    # print(results)  # access the output

    video_writer.write(results.plot_im)  # write the video file

cap.release()
video_writer.release()
cv2.destroyAllWindows()  # destroy all opened windows
```

Example 3 (unknown):
```unknown
yolo solutions trackzone source="path/to/video.mp4" show=True
```

Example 4 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))

# Define region points
region_points = [(150, 150), (1130, 150), (1130, 570), (150, 570)]

# Video writer
video_writer = cv2.VideoWriter("object_counting_output.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Init trackzone (object tracking in zones, not complete frame)
trackzone = solutions.TrackZone(
    show=True,  # display the output
    region=region_points,  # pass region points
    model="yolo11n.pt",
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()
    if not success:
        print("Video frame is empty or video processing has been successfully completed.")
        break
    results = trackzone(im0)
    video_writer.write(results.plot_im)

cap.release()
video_writer.release()
cv2.destroyAllWindows()
```

---

## 使用 Ultralytics YOLO11 进行分析 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/analytics/

**Contents:**
- 使用 Ultralytics YOLO11 进行分析
- 简介
  - 可视化示例
  - 为什么图表很重要
  - Analytics 参数
- 结论
- 常见问题
  - 如何使用 Ultralytics YOLO11 Analytics 创建折线图？
  - 使用 Ultralytics YOLO11 创建条形图有哪些好处？
  - 为什么我应该使用 Ultralytics YOLO11 在我的数据可视化项目中创建饼图？

本指南全面概述了三种基本类型的 数据可视化：折线图、条形图和饼图。每个部分都包含关于如何使用 python 创建这些可视化的分步说明和代码片段。

观看： 如何使用 Ultralytics 生成分析图表 | 折线图、条形图、面积图和饼图

使用 Ultralytics YOLO 进行分析

以下表格概述了 Analytics 参数：

您还可以利用不同的 track 中的参数 Analytics 解决方案。

了解何时以及如何使用不同类型的可视化对于有效的数据分析至关重要。折线图、条形图和饼图是基本工具，可以帮助您更清晰有效地传达数据的故事。Ultralytics YOLO11 Analytics 解决方案提供了一种简化的方法，可以从您的对象检测和跟踪结果中生成这些可视化，从而更容易从您的视觉数据中提取有意义的见解。

要使用 Ultralytics YOLO11 Analytics 创建折线图，请按照以下步骤操作：

有关配置 Analytics 类的更多详细信息，请访问 使用 Ultralytics YOLO11 进行分析 部分。

使用 Ultralytics YOLO11 创建条形图具有以下几个优点：

要了解更多信息，请访问指南中的条形图部分。

Ultralytics YOLO11 是创建饼图的绝佳选择，因为：

是的，Ultralytics YOLO11 可用于 track 对象并动态更新可视化。它支持实时 track 多个对象，并可根据被 track 对象的数据更新各种可视化，例如折线图、条形图和饼图。

Ultralytics YOLO11 因多种原因在 OpenCV 和 TensorFlow 等其他目标检测解决方案中脱颖而出：

有关更详细的比较和用例，请浏览我们的 Ultralytics 博客。

**Examples:**

Example 1 (markdown):
```markdown
yolo solutions analytics show=True

# Pass the source
yolo solutions analytics source="path/to/video.mp4"

# Generate the pie chart
yolo solutions analytics analytics_type="pie" show=True

# Generate the bar plots
yolo solutions analytics analytics_type="bar" show=True

# Generate the area plots
yolo solutions analytics analytics_type="area" show=True
```

Example 2 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
out = cv2.VideoWriter(
    "analytics_output.avi",
    cv2.VideoWriter_fourcc(*"MJPG"),
    fps,
    (1280, 720),  # this is fixed
)

# Initialize analytics object
analytics = solutions.Analytics(
    show=True,  # display the output
    analytics_type="line",  # pass the analytics type, could be "pie", "bar" or "area".
    model="yolo11n.pt",  # path to the YOLO11 model file
    # classes=[0, 2],  # display analytics for specific detection classes
)

# Process video
frame_count = 0
while cap.isOpened():
    success, im0 = cap.read()
    if success:
        frame_count += 1
        results = analytics(im0, frame_count)  # update analytics graph every frame

        # print(results)  # access the output

        out.write(results.plot_im)  # write the video file
    else:
        break

cap.release()
out.release()
cv2.destroyAllWindows()  # destroy all opened windows
```

Example 3 (sql):
```sql
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))

out = cv2.VideoWriter(
    "ultralytics_analytics.avi",
    cv2.VideoWriter_fourcc(*"MJPG"),
    fps,
    (1280, 720),  # this is fixed
)

analytics = solutions.Analytics(
    analytics_type="line",
    show=True,
)

frame_count = 0
while cap.isOpened():
    success, im0 = cap.read()
    if success:
        frame_count += 1
        results = analytics(im0, frame_count)  # update analytics graph every frame
        out.write(results.plot_im)  # write the video file
    else:
        break

cap.release()
out.release()
cv2.destroyAllWindows()
```

Example 4 (sql):
```sql
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))

out = cv2.VideoWriter(
    "ultralytics_analytics.avi",
    cv2.VideoWriter_fourcc(*"MJPG"),
    fps,
    (1280, 720),  # this is fixed
)

analytics = solutions.Analytics(
    analytics_type="bar",
    show=True,
)

frame_count = 0
while cap.isOpened():
    success, im0 = cap.read()
    if success:
        frame_count += 1
        results = analytics(im0, frame_count)  # update analytics graph every frame
        out.write(results.plot_im)  # write the video file
    else:
        break

cap.release()
out.release()
cv2.destroyAllWindows()
```

---

## Reference for ultralytics/trackers/basetrack.py

**URL:** https://docs.ultralytics.com/zh/reference/trackers/basetrack/

**Contents:**
- Reference for ultralytics/trackers/basetrack.py
- class ultralytics.trackers.basetrack.TrackState
- class ultralytics.trackers.basetrack.BaseTrack
  - property ultralytics.trackers.basetrack.BaseTrack.end_frame
  - method ultralytics.trackers.basetrack.BaseTrack.activate
  - method ultralytics.trackers.basetrack.BaseTrack.mark_lost
  - method ultralytics.trackers.basetrack.BaseTrack.mark_removed
  - method ultralytics.trackers.basetrack.BaseTrack.next_id
  - method ultralytics.trackers.basetrack.BaseTrack.predict
  - method ultralytics.trackers.basetrack.BaseTrack.reset_id

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/trackers/basetrack.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Enumeration class representing the possible states of an object being tracked.

Base class for object tracking, providing foundational attributes and methods.

Return the ID of the most recent frame where the object was tracked.

Activate the track with provided arguments, initializing necessary attributes for tracking.

Mark the track as lost by updating its state to TrackState.Lost.

Mark the track as removed by setting its state to TrackState.Removed.

Increment and return the next unique global track ID for object tracking.

Predict the next state of the track based on the current state and tracking model.

Reset the global track ID counter to its initial value.

Update the track with new observations and data, modifying its state and attributes accordingly.

**Examples:**

Example 1 (unknown):
```unknown
TrackState()
```

Example 2 (python):
```python
>>> state = TrackState.New
>>> if state == TrackState.New:
...     print("Object is newly detected.")
```

Example 3 (python):
```python
class TrackState:
```

Example 4 (unknown):
```unknown
BaseTrack(self)
```

---
