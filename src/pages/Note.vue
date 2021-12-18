<template>
  <q-page padding>
    <Container>
      <div v-if="isEditing">
        <form @submit="isEditing = false">
          <q-input v-model="note.title" filled label="Title" />

          <q-input
            v-model="note.description"
            label="Description"
            class="q-mt-s"
            filled
            dense
          />

          <q-card>
            <q-editor v-model="note.content" min-height="5rem" />
          </q-card>

          <div class="q-mt-md">
            <q-btn class="q-mt-sm" color="positive" type="submit">Done</q-btn>
          </div>
        </form>
      </div>

      <div v-else>
        <div class="row items-center justify-between">
          <h3 class="q-mb-md q-mt-md">{{ note.title }}</h3>
          <div>
            <q-btn
              round
              color="secondary"
              icon="edit"
              @click="isEditing = true"
            />
            <q-btn
              class="q-ml-sm"
              round
              color="red"
              icon="delete"
              @click="remove"
            />
          </div>
        </div>

        <div>{{ note.description }}</div>
        <div class="q-mt-md" v-html="note.content" />
      </div> </Container
  ></q-page>
</template>

<script>
import { defineComponent, computed, ref } from "vue";
import { useRoute, useRouter } from "vue-router";
import { useLocalNotes } from "../helper";

import Container from "../components/Container.vue";

export default defineComponent({
  name: "Note",
  components: {
    Container,
  },
  setup() {
    const notes = useLocalNotes();
    const route = useRoute();
    const noteId = computed(() => parseInt(route.params.id));
    const note = computed(() => notes.value[noteId.value]);

    const router = useRouter();

    const remove = () => {
      notes.value.splice(noteId.value, 1);
      router.push("/");
    };

    const isEditing = ref(false);

    return { note, remove, isEditing };
  },
});
</script>

<style lang="scss" scoped></style>
