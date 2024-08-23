gpt 아카이브에 저장되어있음..
이미지 업로드 후 이미지 위에 8핀(핸들러)를 가장자리에 추가하고
이미지위로 z 위치 변환을 주어서 그릴수있는 영역을 추가하여 개발함


```
imageResizeHandlerBind: function(canvasId) {
    var canvas = document.getElementById(canvasId);
    var context = canvas.getContext("2d");
    let scale = 1;
    let dragging = false;
    let handlesCreated = false; // Flag to track if handles are created

    const handlePositions = [
        [0, 0], [canvas.width / 2, 0], [canvas.width, 0],
        [0, canvas.height / 2], [canvas.width, canvas.height / 2],
        [0, canvas.height], [canvas.width / 2, canvas.height], [canvas.width, canvas.height]
    ];

    const handleDrag = (event) => {
        if (dragging) {
            const rect = canvas.getBoundingClientRect();
            const x = event.clientX - rect.left;
            const y = event.clientY - rect.top;
            scale = x / canvas.width;
            resizeCanvas();
            positionHandles();
        }
    };

    const resizeCanvas = () => {
        const newWidth = canvas.width * scale;
        const newHeight = canvas.height * scale;
        canvas.width = newWidth;
        canvas.height = newHeight;
        context.clearRect(0, 0, newWidth, newHeight); // Clear canvas
        positionHandles();
    };

    const positionHandles = () => {
        if (!handlesCreated) {
            handlePositions.forEach((position) => {
                const [x, y] = position;
                const handleSize = 20; // Increase handle size
                context.fillStyle = "#00f"; // Handle color
                context.fillRect(x - handleSize / 2, y - handleSize / 2, handleSize, handleSize);
            });
            handlesCreated = true; // Set the flag to true after creating handles
        }
    };

    const handleClick = (event) => {
        event.stopPropagation(); // Prevent event from propagating to other functions
        // Your handling logic for handle click here
    };

    canvas.addEventListener('mousedown', (event) => {
        event.preventDefault();
        dragging = true;
        window.addEventListener('mousemove', handleDrag);
        window.addEventListener('mouseup', () => {
            dragging = false;
            window.removeEventListener('mousemove', handleDrag);
        });
    });

    // Attach handle click event listeners
    const handleElements = document.getElementsByClassName('resize-handle');
    for (let i = 0; i < handleElements.length; i++) {
        handleElements[i].addEventListener('click', handleClick);
    }

    // Initial setup
    canvas.width = initialCanvasWidth; // Replace with your initial canvas width
    canvas.height = initialCanvasHeight; // Replace with your initial canvas height
    positionHandles();
},
```




```
$(document).ready(function() {
  const imageContainer = $("#imageContainer");
  const myImage = $("#myImage");
  const resizePoints = $(".resizePoint");
  let isResizing = false;
  let resizeType;
  let initialMouseX, initialMouseY;

  resizePoints.on("mousedown", handleMouseDown);

  // 드래그 중에 브라우저의 기본 드래그 동작을 막습니다.
  $(document).on("dragstart", function(e) {
    e.preventDefault();
  });

  // 다른 이벤트를 비활성화합니다.
  $(document).on("mousemove", handleDocumentMouseMove);
  $(document).on("mouseup", handleDocumentMouseUp);

  function handleMouseDown(e) {
    isResizing = true;
    resizeType = $(this).attr("class").split(" ")[1];
    initialMouseX = e.clientX;
    initialMouseY = e.clientY;

    // 드래그 중에만 해당 엘리먼트의 이벤트를 활성화합니다.
    $(this).on("mousemove", handleMouseMove);
    $(this).on("mouseup", handleMouseUp);
  }

  function handleMouseMove(e) {
    // 드래그 중에만 해당 엘리먼트의 이벤트를 처리합니다.
    if (isResizing) {
      e.preventDefault(); // 브라우저의 기본 드래그 동작을 막습니다.
      const deltaX = e.clientX - initialMouseX;
      const deltaY = e.clientY - initialMouseY;

      // 이후의 코드는 이전과 동일합니다.

      initialMouseX = e.clientX;
      initialMouseY = e.clientY;

      updateResizePoints();
    }
  }

  function handleMouseUp() {
    isResizing = false;

    // 드래그가 끝나면 해당 엘리먼트의 이벤트를 다시 비활성화합니다.
    $(this).off("mousemove");
    $(this).off("mouseup");
  }

  // 다른 이벤트 처리 함수들
  function handleDocumentMouseMove(e) {
    // 다른 이벤트 처리 로직
  }

  function handleDocumentMouseUp() {
    // 다른 이벤트 처리 로직
  }

  function updateResizePoints() {
    // 업데이트 함수 내용은 이전과 동일하게 유지합니다.
  }
});
```
