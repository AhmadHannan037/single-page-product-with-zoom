<!-- 
System: Single Product page with zoom
Organization: XYZ
Developer:Ahmad Hanan
Date: 10/29/2023
Purpose: This component display the product slider to user
props: currentMedia
-->
<template>
  <!-- Product Slider Component -->
  <div class="relative product-slider bg-white shadow flex items-center justify-center no-select py-10">
    <div>
      <!-- Image/Video Container -->
      <div class="relative" @click="openModal">
        <div v-if="currentMedia.type === 'video'">
          <video
            class="fade-in w-full h-full"
            controls
            autoplay
            @touchstart="handleTouchStart"
            @touchend="handleTouchEnd"
          >
            <source :src="currentMedia.url" type="video/mp4" />
          </video>
        </div>
        <div v-else>
          <img
            class="fade-in h-48 sm:h-96"
            :src="currentMedia.url"
            :alt="currentMedia.alt"
            ref="productImg"
            @mousemove="updateZoomPosition"
            @touchstart="handleTouchStart"
            @touchend="handleTouchEnd"
          />
          <!-- Image Magnifier Lens -->
          <div
            ref="magnifierLens"
            class="magnifier-lens hidden md:block"
            @mousemove="updateZoomPosition"
            @mouseout="leaveZoomPosition"
          ></div>
        </div>
      </div>
      
      <!-- Navigation Arrows -->
      <button
        @click="prevImage"
        class="absolute top-1/2 transform -translate-y-1/2 left-4 text-2xl text-blue-800 cursor-pointer"
      >
        &lt;
      </button>
      <button
        @click="nextImage"
        class="absolute top-1/2 transform -translate-y-1/2 right-4 text-2xl text-blue-800 cursor-pointer"
      >
        &gt;
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    currentMedia: Object, // Pass the current media data as a prop
  },
  methods: {
    // Emit events for parent component to handle
    nextImage() {
      this.$emit("next-image");
    },
    prevImage() {
      this.$emit("prev-image");
    },
    updateZoomPosition(e) {
      this.$emit("update-zoom-position", e);
    },
    leaveZoomPosition() {
      this.$emit("leave-zoom-position");
    },
    handleTouchStart(event) {
      this.$emit("touch-start", event);
    },
    handleTouchEnd(event) {
      this.$emit("touch-end", event);
    },
    openModal() {
      this.$emit("open-modal");
    },
  },
};
</script>
<style scoped>
.product-slide-img {
  height: 400px;
}
.product-slider {
  min-height: 400px !important;
}

.magnifier-lens {
  position: absolute;
  width: 150px;
  height: 150px;
  background-color: #ff000054;
  border: 1px solid #ff000054;
  opacity: 0;
}

.magnifier-lens.active {
  opacity: 1;
}
</style>