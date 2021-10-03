<template>
    <form @submit.prevent="submit">
        <div>&nbsp;</div>
        <div class="form-group">
            <label for="title">Title</label>
            <input type="text" class="form-control" id="title" v-model="title">
        </div>
        <div>&nbsp;</div>
        <div class="form-group">
            <label for="description">Description</label>
            <textarea type="text" class="form-control" id="description" v-model="description"></textarea>
        </div>
        <div>&nbsp;</div>
        <div class="form-group">
            <label>Image</label>
            <div class="input-group">
                <input type="text" v-model="image" hidden>
                <ImageUpload @file-uploaded="image = $event"/>
            </div>
        </div>
        <div class="form-group">
            <label for="price">Price</label>
            <input type="text" class="form-control" id="price" v-model="price">
        </div>
        <div>&nbsp;</div>
        <button class="btn btn-outline-secondary">Save</button>
    </form>
</template>

<script lang="ts">
import {ref} from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
import ImageUpload from '@/secure/components/ImageUpload.vue';

export default {
    name: 'ProductsCreate',
    components: {
        ImageUpload
    },
    setup() {
        const title = ref('');
        const description = ref('');
        const image = ref('');
        const price = ref('');
        const router = useRouter();

        const submit = async () => {
            await axios.post('/products', {
                title: title.value,
                description: description.value,
                image: image.value,
                price: price.value,
            });

            await router.push('/products');
        }


        return {
            title,
            description,
            image,
            price,
            submit
        }
    }
}
</script>