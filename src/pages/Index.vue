<template>
  <q-page>
    <q-list class="row">
      <q-item
        class="col-xs-12 col-sm-6 col-md-4 col-lg-4"
        v-for="p in posts"
        :key="p.id"
      >
        <q-card>
          <q-card-section>
            <div class="text-h6">{{ p.title.substr(0, 50) + '...' }}</div>
            <q-badge class="q-ma-sm" :floating="true" :color="'teal-10'">
              {{ p.userId }}
            </q-badge>
          </q-card-section>

          <q-card-section class="q-pt-none">
            {{ p.body.substr(0, 200) + '...' }}
          </q-card-section>
          <q-card-actions align="right" class="bg-white text-teal">
            <q-btn
              round
              :to="`/post/${p.id}`"
              :color="'teal-10'"
              :icon="'arrow_forward'"
            ></q-btn>
          </q-card-actions>
        </q-card>
      </q-item>
    </q-list>
  </q-page>
</template>

<script lang="ts">
import {
  defineComponent,
  getCurrentInstance,
  onMounted,
  reactive
} from '@vue/composition-api';
import { Posts } from 'src/components/models';

export default defineComponent({
  name: 'PageIndex',
  components: {},
  setup() {
    const axios = getCurrentInstance()?.$axios;

    const posts = reactive<Posts[]>([]);
    onMounted(async () => {
      await axios({
        method: 'get',
        url: 'https://jsonplaceholder.typicode.com/posts'
      }).then(res => {
        posts.push(...res.data);
      });
    });

    return { posts };
  }
});
</script>
