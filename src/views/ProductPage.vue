<!-- 
System: Single Product page with zoom
Organization: XYZ
Developer:Ahmad Hanan
Date: 10/29/2023
Purpose: This component is the main product component
-->
<template>
  <!-- Main product component -->
  <div class="items-start justify-between lg:flex lg:mt-6">
    <!-- Image Slider and Thumbnails -->
    <div class="flex flex-col justify-center w-full md:mt-3 lg:w-2/5">

      <!-- Product Slider Component -->
      <ProductSlider
        ref="productSlider"
        :currentMedia="product.media.slider[selectedImage]"
        @next-image="nextImage"
        @prev-image="prevImage"
        @update-zoom-position="updateZoomPosition"
        @leave-zoom-position="leaveZoomPosition"
        @touch-start="handleTouchStart"
        @touch-end="handleTouchEnd"
        @open-modal="openModal"
      />

      <!-- Thumbnail Images (Moved below the Image Slider) -->
      <div class="flex justify-center mt-4">

        <!-- Include the ThumbnailList component and pass necessary props -->
        <ProductThumbnail
          :thumbnailImages="product.media.thumbnail"
          :sliderImages="product.media.slider"
          :selectedImage="selectedImage"
          @update:selectedImage="selectedImage = $event"
        />
      </div>
    </div>

    <!-- Product Details and Controls -->
    <div class="relative pt-3 md:pt-1 lg:w-1/2 lg:mt-3 ml-10 lg:ml-0">
      <div ref="magnifiedImage" class="magnified-image hidden md:block"></div>

      <!-- Product Description Component -->
      <ProductDescription :product="product" />

      <div class="pt-4 mt-2">
        <!-- Quantity Control -->
        <div class="flex items-center justify-center sm:justify-start">
          <p
            class="text-5xl mr-2 text-gray-600 cursor-pointer"
            @click="decreaseQuantity"
          >
            -
          </p>
          <div
            class="px-4 py-2 text-blue-800 font-bold bg-gray-200 lg:px-6 lg:py-3 no-select"
          >
            {{ quantity }}
          </div>
          <p
            class="text-4xl ml-2 text-gray-600 cursor-pointer"
            @click="quantity++"
          >
            +
          </p>
        </div>
      </div>
      <div
        class="flex flex-col items-center justify-between w-full mt-4 lg:flex-row"
      >
        <!-- Add to Basket Button -->
        <button
          class="w-full rounded py-2 text-lg font-semibold text-black bg-yellow-300 focus:outline-none lg:px-8 xl:px-16"
          
        >
          Add to Basket
        </button>
      </div>
    </div>

    <!-- Product Popover -->
    <ProductPopover
      v-if="isModalOpen"
      @closeModal="closeModal"
      :product="product"
      :currentMedia="selectedImage"
    />
  </div>
</template>

<script>
import ProductPopover from "@/components/ProductHelpers/ProductPopover";
import ProductDescription from "@/components/ProductHelpers/ProductDescription";
import ProductSlider from "@/components/ProductHelpers/ProductSlider";
import ProductThumbnail from "@/components/ProductHelpers/ProductThumbnail";

export default {
  components: {
    ProductPopover,
    ProductDescription,
    ProductSlider,
    ProductThumbnail,
  },
  data() {
    return {
      isModalOpen: false,
      selectedImage: 0,
      quantity: 1,
      product: {
        brand: "Samsung",
        name: "Samsung Galaxy S23 Plus 5G",
        model: "SM-S916B",
        price: "€880.00",
        status: "Hurry Limited Stock",
        media: {
          slider: [
            {
              id: 1,
              url: require("@/assets/images/product-image.jpeg"),
              alt: "Image 1",
              type: "image",
            },
            {
              id: 2,
              url: require("@/assets/videos/s23-demo.mp4"),
              type: "video",
            },
            {
              id: 3,
              url: require("@/assets/images/product-image-front.jpeg"),
              alt: "Image 3",
              type: "image",
            },
            {
              id: 4,
              url: require("@/assets/images/product-image-back.jpeg"),
              alt: "Image 4",
              type: "image",
            },
          ],
          thumbnail: [
            {
              id: 1,
              url: require("@/assets/images/product-image-thumbnail.jpeg"),
              alt: "Thumbnail 1",
              type: "image",
            },
            {
              id: 2,
              url: require("@/assets/videos/s23-demo.mp4"),
              type: "video",
            },
            {
              id: 3,
              url: require("@/assets/images/product-image-front-thumbnail.jpeg"),
              alt: "Thumbnail 2",
              type: "image",
            },
            {
              id: 4,
              url: require("@/assets/images/product-image-back-thumbnail.jpeg"),
              alt: "Thumbnail 3",
              type: "image",
            },
          ],
        },
        sim: "dual sim",
        condition: "like new",
        colour: "lavender",
        network: "unlocked",
        storage: "512gb",
      },
      touchstartX: 0,
      touchstartY: 0,
      touchendX: 0,
      touchendY: 0,
    };
  },

  methods: {
    /**
     * Navigate to the next image in the slider.
     *
     * @param {void}
     * @return {void}
     */

    nextImage() {
      this.selectedImage =
        (this.selectedImage + 1) % this.product.media.slider.length;
    },

    /**
     * Navigate to the previous image in the slider.
     *
     * @param {void}
     * @return {void}
     */
    prevImage() {
      this.selectedImage =
        (this.selectedImage - 1 + this.product.media.slider.length) %
        this.product.media.slider.length;
    },

    /**
     * Update the zoom position of the product image based on mouse/touch events.
     *
     * @param {Event} e - Mouse or touch event
     * @return {void}
     */
    updateZoomPosition(e) {
      // Calculate the coordinates and sizes needed for zoom
      let x, y, cx, cy;

      // Get the bounding rectangle of the product image
      const productImgRect =
        this.$refs.productSlider.$refs.productImg.getBoundingClientRect();

      // Calculate the position of the magnifier lens
      x =
        e.pageX -
        productImgRect.left -
        this.$refs.productSlider.$refs.magnifierLens.offsetWidth / 2;
      y =
        e.pageY -
        productImgRect.top -
        this.$refs.productSlider.$refs.magnifierLens.offsetHeight / 2;

      // Ensure the magnifier lens stays within the image bounds
      let maxXpos =
        productImgRect.width -
        this.$refs.productSlider.$refs.magnifierLens.offsetWidth;
      let maxYpos =
        productImgRect.height -
        this.$refs.productSlider.$refs.magnifierLens.offsetHeight;

      if (x > maxXpos) {
        x = maxXpos;
      }
      if (x < 0) {
        x = 0;
      }

      if (y > maxYpos) {
        y = maxYpos;
      }
      if (y < 0) {
        y = 0;
      }

      // Update the magnifier lens position
      this.$refs.productSlider.$refs.magnifierLens.style.cssText = `top: ${y}px; left: ${x}px;`;

      // Calculate the zoomed image coordinates
      cx =
        this.$refs.magnifiedImage.offsetWidth /
        this.$refs.productSlider.$refs.magnifierLens.offsetWidth;
      cy =
        this.$refs.magnifiedImage.offsetHeight /
        this.$refs.productSlider.$refs.magnifierLens.offsetHeight;

      // Update the magnified image background position for zoom effect
      this.$refs.magnifiedImage.style = `background: url(${
        this.$refs.productSlider.$refs.productImg.src
      })
                                          -${x * cx}px -${y * cy}px /${
        productImgRect.width * cx
      }px ${productImgRect.height * cy}px
                                           no-repeat`;

      // Add the 'active' class to show the magnifier lens and magnified image
      this.$refs.productSlider.$refs.magnifierLens.classList.add("active");
      this.$refs.magnifiedImage.classList.add("active");
    },

    /**
     * Reset the zoom position and hide the magnified image.
     *
     * @param {void}
     * @return {void}
     */
    leaveZoomPosition() {
      this.$refs.productSlider.$refs.magnifierLens.classList.remove("active");
      this.$refs.magnifiedImage.classList.remove("active");
    },

    decreaseQuantity() {
      if (this.quantity > 1) {
        this.quantity--;
      }
    },
    handleTouchStart(event) {
      this.touchstartX = event.changedTouches[0].screenX;
      this.touchstartY = event.changedTouches[0].screenY;
    },
    handleTouchEnd(event) {
      this.touchendX = event.changedTouches[0].screenX;
      this.touchendY = event.changedTouches[0].screenY;
      this.handleGesture();
    },
    openModal() {
      this.isModalOpen = true;
    },
    closeModal() {
      this.isModalOpen = false;
    },

    /**
     * Handle swipe gestures to navigate images.
     *
     * @param {void}
     * @return {void}
     */
    handleGesture() {
      //swipe left
      if (this.touchendX < this.touchstartX) {
        this.nextImage();
      }
      //swipe right
      if (this.touchendX > this.touchstartX) {
        this.prevImage();
      }
    },
  },
};
</script>

<style>
.thumbnail-image {
  transition: all 0.2s;
}

.thumbnail-image:hover {
  transform: scale(1.1);
}

.magnified-image {
  position: absolute;
  width: 100%;
  height: 100%;
  border: 2px solid rgb(232, 99, 99);
  opacity: 0;
  transform: scale(0.5);
  transition: opacity 0.5s, transform 0.5s;
}

.magnified-image.active {
  opacity: 1;
  transform: scale(1);
}
</style>
