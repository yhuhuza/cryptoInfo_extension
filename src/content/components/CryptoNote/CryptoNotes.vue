<template>
  <div>
    <div class="note-pattern">
      <div class="note-editing">
        <font-awesome-icon class="edit-note" @click="editNote" :icon="['fas', 'pen']"></font-awesome-icon>
        <font-awesome-icon class="delete-note" @click="deleteNote(item.id)" :icon="['fas', 'trash-can']"></font-awesome-icon>
      </div>
      <div>
        <h3 class="note-title">{{ item.title }}</h3>
        <p class="note-description">{{ item.description }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex';
import { PAGES } from '../../../utilities/constants';
import { changeTab } from "@/utilities/mixins";

export default {
  data() {
    return {
      pages: PAGES,
      noteDescription: this.item.description,
      titleNote: this.item.title,
    }
  },
  props: {
    item: {
      type: Object,
      default: () => ({}),
    }
  },
  mixins: [
    changeTab,
  ],
  methods: {
    deleteNote(note) {
      chrome.runtime.sendMessage({
        action: 'multiFunc',
        data: {
          note,
          act: 'deleteNote',
          storeAct: 'getNote',
        }
      });
    },
    editNote() {
      this.$store.commit('set', {
        editNote:  this.item.id,
        }
      );
      this.changeTab(this.pages.add);
    },
  },
  computed: {
    ...mapState([
        'cryptoNotes'
    ]),
    checkForDisplay() {
      return (this.item.title && this.item.description);
    }
  }
}
</script>

<style scoped>

.note-pattern {
  color: white;
  margin: 0 30px 10px;
  width: 300px;
  padding: 5px;
  border: 2px solid orange;
  border-radius: 5px;
}

.note-title {
  display: flex;
  justify-content: center;
  margin: 0 0 20px;
  font-size: 20px;
}

.note-description {
  margin: 0 20px;
  font-size: 14px;
  overflow: hidden;
  overflow-wrap: break-word;
  text-align: center;
}

.note-editing {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 10px 10px 0;
}

svg {
  height: 20px;
}

svg:hover {
  color: orange;
  transform: scale(1.2);
  transition: 0.5s;
  cursor: pointer;
}

</style>