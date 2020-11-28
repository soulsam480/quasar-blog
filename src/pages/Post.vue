<template>
  <q-page>
    <h4 class="q-pa-sm q-ma-none">{{ post.title }}</h4>
    <p>{{ post.body }}</p>
  </q-page>
</template>

<script lang="ts">
import {
  defineComponent,
  getCurrentInstance,
  onMounted,
  reactive
} from '@vue/composition-api';
import { AxiosResponse } from 'axios';
import { Posts } from 'src/components/models';

export default defineComponent({
  name: 'Post',
  setup() {
    const route = getCurrentInstance()?.$route;
    const axios = getCurrentInstance()?.$axios;
    const post = reactive<Posts>({});

    onMounted(async () => {
      await axios({
        method: 'get',
        url: `https://jsonplaceholder.typicode.com/posts/${route?.params.id}`
      }).then((res: AxiosResponse<Posts>) => {
        post.body = res.data.body;
        post.id = res.data.id;
        post.userId = res.data.userId;
        post.title = res.data.title;
      });
    });

    return { post };
  }
});
</script>
