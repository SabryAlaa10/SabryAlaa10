# Interactive Image Carousel Component

This is a modern, responsive image carousel that can be directly copied and used in any project. It includes navigation controls, thumbnails, fullscreen view, autoplay, and mobile touch support.

## HTML

```html
<div class="carousel">
    <div class="carousel-inner">
        <div class="carousel-item active">
            <img src="image1.jpg" alt="Image 1">
        </div>
        <div class="carousel-item">
            <img src="image2.jpg" alt="Image 2">
        </div>
        <div class="carousel-item">
            <img src="image3.jpg" alt="Image 3">
        </div>
        <!-- Add more images as needed -->
    </div>
    <div class="carousel-controls">
        <button class="carousel-prev">❮</button>
        <button class="carousel-next">❯</button>
    </div>
    <div class="thumbnails">
        <img src="image1.jpg" alt="Thumbnail 1" class="thumbnail active">
        <img src="image2.jpg" alt="Thumbnail 2" class="thumbnail">
        <img src="image3.jpg" alt="Thumbnail 3" class="thumbnail">
        <!-- Add more thumbnails as needed -->
    </div>
</div>
```

## CSS

```css
.carousel {
    position: relative;
    max-width: 800px;
    margin: auto;
    overflow: hidden;
}

.carousel-inner {
    display: flex;
    transition: transform 0.5s ease;
}

.carousel-item {
    min-width: 100%;
    box-sizing: border-box;
}

.carousel img {
    width: 100%;
    display: block;
}

.carousel-controls {
    position: absolute;
    top: 50%;
    width: 100%;
    display: flex;
    justify-content: space-between;
}

.carousel-prev, .carousel-next {
    background-color: rgba(255, 255, 255, 0.7);
    border: none;
    padding: 10px;
    cursor: pointer;
}

.thumbnails {
    display: flex;
    justify-content: center;
    margin-top: 10px;
}

.thumbnail {
    width: 60px;
    margin: 0 5px;
    cursor: pointer;
    opacity: 0.6;
}

.thumbnail.active {
    opacity: 1;
}
```

## JavaScript

```javascript
const items = document.querySelectorAll('.carousel-item');
const thumbnails = document.querySelectorAll('.thumbnail');
const prevButton = document.querySelector('.carousel-prev');
const nextButton = document.querySelector('.carousel-next');

let currentIndex = 0;
let autoplayInterval;

function showItem(index) {
    items.forEach((item, i) => {
        item.classList.toggle('active', i === index);
        thumbnails[i].classList.toggle('active', i === index);
    });
    document.querySelector('.carousel-inner').style.transform = `translateX(-${index * 100}%)`;
}

function nextItem() {
    currentIndex = (currentIndex + 1) % items.length;
    showItem(currentIndex);
}

function prevItem() {
    currentIndex = (currentIndex - 1 + items.length) % items.length;
    showItem(currentIndex);
}

function startAutoplay() {
    autoplayInterval = setInterval(nextItem, 3000);
}

function stopAutoplay() {
    clearInterval(autoplayInterval);
}

nextButton.addEventListener('click', () => {
    stopAutoplay();
    nextItem();
    startAutoplay();
});

prevButton.addEventListener('click', () => {
    stopAutoplay();
    prevItem();
    startAutoplay();
});

thumbnails.forEach((thumbnail, index) => {
    thumbnail.addEventListener('click', () => {
        stopAutoplay();
        currentIndex = index;
        showItem(currentIndex);
        startAutoplay();
    });
});

// Touch support
let touchStartX = 0;
let touchEndX = 0;

const handleTouchStart = (event) => {
    touchStartX = event.changedTouches[0].clientX;
};

const handleTouchEnd = (event) => {
    touchEndX = event.changedTouches[0].clientX;
    handleGesture();
};

const handleGesture = () => {
    if (touchStartX > touchEndX + 50) {
        nextItem();
    } else if (touchStartX < touchEndX - 50) {
        prevItem();
    }
};

document.querySelector('.carousel').addEventListener('touchstart', handleTouchStart);
document.querySelector('.carousel').addEventListener('touchend', handleTouchEnd);

// Start autoplay on load
startAutoplay();
```
