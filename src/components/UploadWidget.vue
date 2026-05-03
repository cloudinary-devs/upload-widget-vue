<template>
  <button class="upload-btn" @click="openWidget">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/>
      <polyline points="17 8 12 3 7 8"/>
      <line x1="12" y1="3" x2="12" y2="15"/>
    </svg>
    Upload Files
  </button>
</template>

<script setup>
import { onMounted } from 'vue'

const props = defineProps({
  cloudName: { type: String, required: true },
  uploadPreset: { type: String, required: true },
})

const emit = defineEmits(['uploaded'])

let widget

onMounted(() => {
  widget = cloudinary.createUploadWidget(
    {
      cloudName: props.cloudName,
      uploadPreset: props.uploadPreset,
      sources: ['local', 'url', 'camera'],
      multiple: true,
      // cropping: true,                          // add a cropping step
      // showAdvancedOptions: true,               // add advanced options (public_id and tag)
      // folder: 'user_images',                   // upload files to the specified folder
      // tags: ['users', 'profile'],              // add the given tags to the uploaded files
      // context: { alt: 'user_uploaded' },       // add the given context data to the uploaded files
      // clientAllowedFormats: ['images'],        // restrict uploading to image files only
      // maxImageFileSize: 2000000,               // restrict file size to less than 2MB
      // maxImageWidth: 2000,                     // scales the image down to a width of 2000 pixels before uploading
      // theme: 'purple',                         // change to a purple theme
      styles: {
        palette: {
          window: '#FFFFFF',
          windowBorder: '#90A0B3',
          tabIcon: '#0078FF',
          menuIcons: '#5A616A',
          textDark: '#000000',
          textLight: '#FFFFFF',
          link: '#0078FF',
          action: '#FF620C',
          inactiveTabIcon: '#0E2F5A',
          error: '#F44235',
          inProgress: '#0078FF',
          complete: '#20B832',
          sourceBg: '#E4EBF1',
        },
      },
    },
    (error, result) => {
      if (error) {
        console.error('Upload error:', error)
        return
      }
      if (result?.event === 'success') {
        emit('uploaded', result.info)
      }
    }
  )
})

function openWidget() {
  widget.open()
}
</script>
