<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Trò chơi kéo thả hình</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background: #f5f5f5;
        text-align: center;
        margin: 0;
        padding: 20px;
    }

    h2 {
        margin-bottom: 10px;
    }

    .game-area {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 30px;
    }

    .targets, .items {
        display: flex;
        gap: 30px;
        flex-wrap: wrap;
        justify-content: center;
    }

    .target, .item {
        width: 90px;
        height: 90px;
        touch-action: none;
    }

    /* TARGETS */
    .circle-target {
        border: 4px solid black;
        border-radius: 50%;
        background: white;
    }

    .square-target {
        border: 4px solid black;
        background: white;
    }

    .triangle-target {
        position: relative;
    }

    .triangle-target::before {
        content: "";
        position: absolute;
        left: 50%;
        top: 5px;
        transform: translateX(-50%);
        width: 0;
        height: 0;
        border-left: 40px solid transparent;
        border-right: 40px solid transparent;
        border-bottom: 70px solid black;
    }

    .triangle-target::after {
        content: "";
        position: absolute;
        left: 50%;
        top: 10px;
        transform: translateX(-50%);
        width: 0;
        height: 0;
        border-left: 35px solid transparent;
        border-right: 35px solid transparent;
        border-bottom: 60px solid white;
    }

    /* ITEMS */
    .circle {
        background: red;
        border-radius: 50%;
    }

    .square {
        background: dodgerblue;
    }

    .triangle {
        width: 0;
        height: 0;
        border-left: 45px solid transparent;
        border-right: 45px solid transparent;
        border-bottom: 80px solid orange;
    }

    .dragging {
        opacity: 0.7;
        position: absolute;
        z-index: 1000;
    }
</style>
</head>
<body>

<h2>🎮 Kéo hình vào đúng vị trí</h2>

<div class="game-area">
    <div class="targets">
        <div class="target circle-target" data-type="circle"></div>
        <div class="target square-target" data-type="square"></div>
        <div class="target triangle-target" data-type="triangle"></div>
    </div>

    <div class="items">
        <div class="item circle" data-type="circle"></div>
        <div class="item square" data-type="square"></div>
        <div class="item triangle" data-type="triangle"></div>
    </div>
</div>

<script>
let draggedItem = null;
let offsetX = 0;
let offsetY = 0;
let startX = 0;
let startY = 0;

document.querySelectorAll('.item').forEach(item => {
    item.addEventListener('pointerdown', e => {
        draggedItem = item;
        startX = item.offsetLeft;
        startY = item.offsetTop;
        offsetX = e.clientX - item.getBoundingClientRect().left;
        offsetY = e.clientY - item.getBoundingClientRect().top;
        item.classList.add('dragging');
    });
});

document.addEventListener('pointermove', e => {
    if (!draggedItem) return;
    draggedItem.style.left = (e.clientX - offsetX) + 'px';
    draggedItem.style.top = (e.clientY - offsetY) + 'px';
});

document.addEventListener('pointerup', e => {
    if (!draggedItem) return;

    let matched = false;
    document.querySelectorAll('.target').forEach(target => {
        const tRect = target.getBoundingClientRect();
        const iRect = draggedItem.getBoundingClientRect();

        if (
            iRect.left < tRect.right &&
            iRect.right > tRect.left &&
            iRect.top < tRect.bottom &&
            iRect.bottom > tRect.top &&
            draggedItem.dataset.type === target.dataset.type
        ) {
            draggedItem.style.left = tRect.left + 'px';
            draggedItem.style.top = tRect.top + 'px';
            matched = true;
        }
    });

    if (!matched) {
        draggedItem.style.left = startX + 'px';
        draggedItem.style.top = startY + 'px';
    }

    draggedItem.classList.remove('dragging');
    draggedItem = null;
});
</script>

</body>
</html>
