# cieralteir-vue-image-uploader

## Installation

``` bash
npm install cieralteir-vue-image-uploader
```

## Usage

``` javascript
import CieralteirVueImageUploader from "cieralteir-vue-image-uploader";
Vue.component("cieralteir-vue-image-uploader", CieralteirVueImageUploader);
```

Basic
``` javascript
<cieralteir-vue-image-uploader
    :name="cover"
    :accepted-mime-types="['image/jpeg']"
    :max-file-size="10"
></cieralteir-vue-image-uploader>
```

Multiple
``` javascript
<cieralteir-vue-image-uploader
    :multiple="true"
></cieralteir-vue-image-uploader>
```

Autoupload
``` javascript
<cieralteir-vue-image-uploader
    :apiMode="true"
    upload-url="https://server.com/image/upload"
    :uploads="arrayOfImageObjects"
></cieralteir-vue-image-uploader>
```

## Props

| Prop              | Type          | Default  | Description                                                      |
| ----------------- | ------------- | -------- | ---------------------------------------------------------------- |
| apiMode           | Boolean       | false    | If set to true, autoupload feature will be turned on.            |
| uploadUrl         | String        |          | Server end point to POST request (autoupload).                   |
| uploads           | Array         |          | Array of image objects. Image object should have name (name of the image), source (image source path), deleteUrl (server end point for deleting the image) |
| multiple          | Boolean       | false    | If set to true, file input's multi attribute will be set to true |
| name              | String        | image    | Name attribute of the file input.                                |
| acceptedMimeTypes | Array         |          | Array of mime types to be accepted.                              |
| maxFileSize       | Number        |          | Max file size allowed in megabytes.                              |
