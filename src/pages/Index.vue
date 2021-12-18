<template>
  <q-page padding>
    <Container>
      <div class="row items-center justify-between">
        <h3>Your notes</h3>

        <div>
          <q-btn round color="positive" icon="add" to="/new"></q-btn>
        </div>
      </div>

      <NoteCard
        v-for="({ title, description }, index) in notes"
        :key="index"
        :title="title"
        :description="description"
        @click="router.push(`/note/${index}`)"
      />

      <div v-if="notes.length === 0">You have not created any notes</div>
    </Container>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import { useLocalNotes } from "../helper";
import { useRouter } from "vue-router";

import Container from "../components/Container.vue";
import NoteCard from "../components/NoteCard.vue";

export default defineComponent({
  name: "PageIndex",
  components: {
    Container,
    NoteCard,
  },
  setup() {
    const notes = useLocalNotes();
    const router = useRouter();

    return {
      notes,
      router,
    };
  },
});
</script>
