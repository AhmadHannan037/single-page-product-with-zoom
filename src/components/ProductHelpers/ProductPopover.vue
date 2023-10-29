<!-- 
System: Single Product page with zoom
Organization: XYZ
Developer:Ahmad Hanan
Date: 10/29/2023
Purpose: This component display the product popupover to user
props: product (Object)
-->
<template>
  <!-- Product Modal -->
  <div
    class="fixed inset-0 flex items-center justify-center z-50 bg-black bg-opacity-50 m-0 sm:m-3 border-solid border-2 border-dark-100"
  >
    <div class="bg-white w-full h-full p-8 relative">
      <!-- Close button -->
      <button
        @click="closeModal"
        class="absolute top-2 right-2 text-gray-500 hover:text-gray-700"
      >
        <!-- Close icon -->
        <svg
          class="w-6 h-6"
          fill="none"
          stroke="currentColor"
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          viewBox="0 0 24 24"
        >
          <path d="M6 18L18 6M6 6l12 12"></path>
        </svg>
      </button>

      <!-- Tab buttons for Images and Videos -->
      <div class="flex border-solid border-b-2 border-slate-300">
        <div class="w-1/2 sm:w-1/12">
          <button
            :class="{
              'text-slate-900 border-solid border-b-2 border-slate-300':
                activeTab === 'image',
              'text-slate-400': activeTab !== 'image',
            }"
            @click="changeTab('image')"
            class="w-24 py-2 focus:outline-none bg-transparent"
          >
            Images
          </button>
        </div>
        <div class="w-1/2 sm:w-1/12">
          <button
            :class="{
              'text-slate-900 border-solid border-b-2 border-slate-300':
                activeTab === 'video',
              'text-slate-400': activeTab !== 'video',
            }"
            @click="changeTab('video')"
            class="w-24 py-2 focus:outline-none bg-transparent"
          >
            Videos
          </button>
        </div>
      </div>

      <!-- Media Display Section (Images or Videos) -->
      <div class="p-4 flex">
        <div class="w-full sm:w-2/3 text-center">
          <!-- Product title -->
          <h4 class="text-xs font-bold mb-4 block md:hidden">
            {{ product.name }} {{ product.price }} Sim:
            {{ product.sim }} Condition: {{ product.condition }} Colour:
            {{ product.colour }} Network: {{ product.network }} Storage:
            {{ product.storage }}
          </h4>
          <!-- Display Images -->
          <img
            v-if="activeTab === 'image'"
            :src="product.media.slider[selectedImage].url"
            :alt="product.media.slider[selectedImage].alt"
            class="mx-auto h-48 sm:h-96 sm:max-h-96 sm:max-w-full"
            @click="toggleZoomedIn"
            :class="{ 'zoomed-in': isZoomedIn, 'zoomed-out': !isZoomedIn }"
          />
          <!-- Display Videos -->
          <video
            v-else-if="activeTab === 'video'"
            class="mx-auto max-h-96 max-w-full"
            controls
            autoplay
          >
            <source
              :src="product.media.slider[selectedImage].url"
              type="video/mp4"
            />
          </video>
        </div>

        <!-- Thumbnails Container -->
        <div class="w-0 sm:w-1/3 p-0 sm:p-4">
          <!-- Product Title on the Top Right -->
          <h2 class="text-xl font-bold mb-4 hidden sm:block">
            {{ product.name }} {{ product.price }} Sim:
            {{ product.sim }} Condition: {{ product.condition }} Colour:
            {{ product.colour }} Network: {{ product.network }} Storage:
            {{ product.storage }}
          </h2>

          <!-- Thumbnails for Images -->
          <div
            v-if="activeTab === 'image'"
            class="sm:hidden fixed bottom-0 left-0 right-0 bg-white p-4 shadow-md"
          >
            <ProductThumbnail
              :thumbnailImages="imageMedia"
              :sliderImages="product.media.slider"
              :selectedImage="selectedImage"
              @update:selectedImage="selectedImage = $event"
            />
          </div>
          <div v-if="activeTab === 'image'" class="hidden sm:block">
            <ProductThumbnail
              :thumbnailImages="imageMedia"
              :sliderImages="product.media.slider"
              :selectedImage="selectedImage"
              @update:selectedImage="selectedImage = $event"
            />
          </div>

          <!-- Thumbnails for Videos -->
          <div
            v-if="activeTab === 'video'"
            class="sm:hidden fixed bottom-0 left-0 right-0 bg-white p-4 shadow-md"
          >
            <ProductThumbnail
              :thumbnailImages="videoMedia"
              :sliderImages="product.media.slider"
              :selectedImage="selectedImage"
              @update:selectedImage="selectedImage = $event"
            />
          </div>
          <div v-if="activeTab === 'video'" class="hidden sm:block">
            <ProductThumbnail
              :thumbnailImages="videoMedia"
              :sliderImages="product.media.slider"
              :selectedImage="selectedImage"
              @update:selectedImage="selectedImage = $event"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ProductThumbnail from "@/components/ProductHelpers/ProductThumbnail"; // Import the ProductThumbnail component

export default {
  props: {
    product: Object,
    currentMedia: Number,
  },
  components: {
    ProductThumbnail, // Register the ProductThumbnail component
  },
  data() {
    return {
      activeTab: "image",
      selectedImage: 0,
      isZoomedIn: false,
    };
  },
  computed: {
    /**
     * Returns only array of videos after filtering slider array.
     *
     * @param {void}
     * @return {array} videos array from slider.
     */
    videoMedia() {
      return this.product.media.slider.filter(
        (media) => media.type === "video"
      );
    },

    /**
     * Returns only array of images after filtering slider array.
     *
     * @param {void}
     * @return {array} images array from slider.
     */
    imageMedia() {
      return this.product.media.slider.filter(
        (media) => media.type === "image"
      );
    },
  },
  methods: {
    closeModal() {
      this.$emit("closeModal");
    },
    toggleZoomedIn() {
      this.isZoomedIn = !this.isZoomedIn;
    },

    /**
     * select tab on click and set require properties
     *
     * @param {String} media
     * @return {void}
     */
    changeTab(media) {
      this.activeTab = media;

      if (media === "image") {
        // Find the index of the first image in the slider
        this.selectedImage = this.product.media.slider.findIndex(
          (media) => media.type === "image"
        );
      } else if (media === "video") {
        // Find the index of the first video in the slider
        this.selectedImage = this.product.media.slider.findIndex(
          (media) => media.type === "video"
        );
      }
    },

    //vue lifecyle hook
    created() {
      //set the active tab accordingly
      this.selectedImage = this.currentMedia;
      if (this.product.media.slider[this.currentMedia].type === "image") {
        this.activeTab = "image";
      } else {
        this.activeTab = "video";
      }
    },
  },
};
</script>

<style scoped>
.zoomed-in {
  cursor: zoom-out;
  max-width: 100%;
  max-height: 100%;
  width: auto;
  height: auto;
  transition: transform 0.3s; /* Smooth zoom effect */
  transform: scale(1.3); /* Zoom scale factor, adjust as needed */
  z-index: 100; /* Ensures the zoomed-in image is on top */
}
.zoomed-out {
  cursor: zoom-in;
}
</style>
