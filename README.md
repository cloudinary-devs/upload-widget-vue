# Cloudinary Upload Widget — Vue Example

A minimal Vue 3 app demonstrating how to integrate the [Cloudinary Upload Widget](https://cloudinary.com/documentation/upload_widget) using plain JavaScript (no Vue SDK required).

## What this demo covers

- Loading the Upload Widget from Cloudinary's CDN
- Creating the widget once in `onMounted` with `cloudinary.createUploadWidget()`
- Opening the widget on button click with `widget.open()`
- Handling the result callback and emitting the result to the parent via `defineEmits`
- Displaying uploaded files in a reactive gallery using `ref()`

## Prerequisites

- [Node.js](https://nodejs.org/) 18+
- A Cloudinary account — [sign up free](https://cloudinary.com/users/register_free)

## Getting started

1. **Clone the repo**

   ```bash
   git clone https://github.com/cloudinary-devs/upload-widget-vue.git
   cd upload-widget-vue
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Run the demo as-is, or use your own credentials**

   The app is pre-configured with a working cloud name and unsigned upload preset so it runs immediately out of the box. When you're ready to use your own Cloudinary account, open `src/App.vue` and update the two constants at the top of `<script setup>`:

   ```javascript
   const cloudName = 'your_cloud_name'
   const uploadPreset = 'your_upload_preset'
   ```

   > Your upload preset must be set to **unsigned** in your Cloudinary console under **Settings → Upload → Upload presets**.

4. **Run the app**

   ```bash
   npm run dev
   ```

   Open [http://localhost:5173](http://localhost:5173) in your browser.

## How it works

The Upload Widget script is loaded globally in `index.html`:

```html
<script src="https://upload-widget.cloudinary.com/latest/global/all.js" type="text/javascript"></script>
```

In `UploadWidget.vue`, the widget is created once in `onMounted` and reused on every button click. A successful upload emits an `uploaded` event to the parent:

```javascript
onMounted(() => {
  widget = cloudinary.createUploadWidget(
    { cloudName: props.cloudName, uploadPreset: props.uploadPreset },
    (error, result) => {
      if (!error && result?.event === 'success') {
        emit('uploaded', result.info)
      }
    }
  )
})
```

`App.vue` listens for the event and prepends the result to a reactive `uploads` array:

```javascript
const uploads = ref([])

function onUploaded(info) {
  uploads.value = [info, ...uploads.value]
}
```

## Configuration options

`src/components/UploadWidget.vue` includes a set of commented-out options you can uncomment to explore widget features:

| Option | Description |
|---|---|
| `cropping: true` | Adds an interactive crop step before upload |
| `showAdvancedOptions: true` | Exposes public ID and tag fields |
| `folder: 'user_images'` | Uploads into a specific folder |
| `tags: ['users', 'profile']` | Applies tags to every uploaded asset |
| `context: { alt: 'user_uploaded' }` | Attaches structured context metadata |
| `clientAllowedFormats: ['images']` | Restricts uploads to image files |
| `maxImageFileSize: 2000000` | Rejects files larger than 2 MB |
| `maxImageWidth: 2000` | Downscales images to 2000 px before upload |
| `theme: 'purple'` | Switches to the built-in purple theme |

Full configuration reference: [Upload Widget parameters](https://cloudinary.com/documentation/upload_widget_reference)

## Project structure

```
src/
├── App.vue                   # Page layout, uploads state, results gallery
└── components/
    └── UploadWidget.vue      # Widget creation, button, emits uploaded event
index.html                    # Loads the Upload Widget script from CDN
```

## Resources

- [Upload Widget documentation](https://cloudinary.com/documentation/upload_widget)
- [Upload Widget reference](https://cloudinary.com/documentation/upload_widget_reference)
- [Vue integration guide](https://cloudinary.com/documentation/vue_integration)
- [Cloudinary Console](https://console.cloudinary.com/)
