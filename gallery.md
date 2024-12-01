---
title: Галерия
description: За ЦПО Пламка Илиева
---
<style>
    /* Grid Styling */
    .image-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 10px;
      justify-items: center;
      max-width: 1200px;
      margin: 20px auto;
    }
    .image-grid img {
      width: 100%;
      height: 200px;
      object-fit: cover;
      cursor: pointer;
      border-radius: 5px;
    }

    /* Modal Styling */
    .modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.8);
      justify-content: center;
      align-items: center;
    }
    .modal img {
      max-width: 90%;
      max-height: 80%;
      border-radius: 5px;
    }
    .modal .close {
      position: absolute;
      top: 20px;
      right: 30px;
      font-size: 2rem;
      color: white;
      cursor: pointer;
    }
    .modal .arrow {
      position: absolute;
      top: 50%;
      font-size: 3rem;
      color: white;
      cursor: pointer;
      transform: translateY(-50%);
      user-select: none;
    }
    .modal .arrow.left {
      left: 20px;
    }
    .modal .arrow.right {
      right: 20px;
    }
</style>
<div class="image-grid">
    <img src="../pics/IMG_20210819_195853.jpg" alt="Image 1" />
    <img src="../pics/IMG_4019.JPG" alt="Image 2" />
    <img src="../pics/IMG_20210819_201604.jpg" alt="Image 3" />
    <img src="../pics/IMG_20210610_173945.jpg" alt="Image 4" />
    <img src="../pics/IMG_20210610_173957.jpg" alt="Image 5" />
    <img src="../pics/IMG_20210610_174105.jpg" alt="Image 6" />
    <img src="../pics/IMG_20210610_174522.jpg" alt="Image 7" />
    <img src="../pics/IMG_20210610_174908.jpg" alt="Image 8" />
    <img src="../pics/IMG_0015.JPG" alt="Image 9" />
    <img src="../pics/IMG_0016.JPG" alt="Image 10" />
    <img src="../pics/IMG_0237.JPG" alt="Image 11" />
    <img src="../pics/IMG_20210819_182606.jpg" alt="Image 12" />
    <img src="../pics/IMG_20210819_182941.jpg" alt="Image 13" />
    <img src="../pics/IMG_20210819_184733.jpg" alt="Image 14" />
    <img src="../pics/IMG_20210819_185545.jpg" alt="Image 15" />
    <img src="../pics/IMG_20210819_200746.jpg" alt="Image 16" />
    <img src="../pics/IMG_0268.JPG" alt="Image 17" />
    <img src="../pics/STA61130.JPG" alt="Image 18" />
    <img src="../pics/STA62132.JPG" alt="Image 19" />
    <img src="../pics/STA63027.JPG" alt="Image 20" />
</div>

<!-- Modal -->
<div class="modal" id="imageModal">
  <span class="close" id="closeModal">&times;</span>
  <span class="arrow left" id="prevImage">&#10094;</span>
  <img id="modalImage" src="" alt="Expanded Image">
  <span class="arrow right" id="nextImage">&#10095;</span>
</div>

<script>
  // JavaScript for Modal
  const modal = document.getElementById('imageModal');
  const modalImage = document.getElementById('modalImage');
  const closeModal = document.getElementById('closeModal');
  const prevImage = document.getElementById('prevImage');
  const nextImage = document.getElementById('nextImage');
  const images = document.querySelectorAll('.image-grid img');

  let currentIndex = 0;

  // Open modal and display the clicked image
  images.forEach((image, index) => {
    image.addEventListener('click', () => {
      currentIndex = index;
      modalImage.src = image.src;
      modal.style.display = 'flex';
    });
  });

  // Close modal
  closeModal.addEventListener('click', () => {
    modal.style.display = 'none';
  });

  // Navigate to the previous image
  prevImage.addEventListener('click', () => {
    currentIndex = (currentIndex - 1 + images.length) % images.length;
    modalImage.src = images[currentIndex].src;
  });

  // Navigate to the next image
  nextImage.addEventListener('click', () => {
    currentIndex = (currentIndex + 1) % images.length;
    modalImage.src = images[currentIndex].src;
  });

  // Close modal when clicking outside the image
  modal.addEventListener('click', (e) => {
    if (e.target === modal) {
      modal.style.display = 'none';
    }
  });

  // Keyboard navigation for left/right arrows
  document.addEventListener('keydown', (e) => {
    if (modal.style.display === 'flex') {
      if (e.key === 'ArrowLeft') {
        prevImage.click();
      } else if (e.key === 'ArrowRight') {
        nextImage.click();
      } else if (e.key === 'Escape') {
        modal.style.display = 'none';
      }
    }
  });
</script>

