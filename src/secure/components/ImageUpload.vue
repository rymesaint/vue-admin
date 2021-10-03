<template>
    <input id="image" type="file" class="form-control" @change="upload($event.target.files)">
    <label for="image" class="input-group-text">Upload</label>
</template>

<script>
import axios from 'axios';
export default {
    name: 'ImageUpload',
    emits: ['file-uploaded'],
    setup(_, {emit}) {
        const upload = async (files) => {
            const file = files.item(0);
            
            const data = new FormData;
            data.append('image', file);

            const response = await axios.post('/upload', data);

            emit('file-uploaded', response.data.url);
        }

        return {
            upload
        }
    }
}
</script>