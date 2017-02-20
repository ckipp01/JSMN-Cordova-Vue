<template>
  <div class="container">
    <div class="body">
      <!-- Sidebar -->
      <div class="sideBar">
        <p> JS | MN </p>
        <button type="button"
        @click="show = 'all'"
        :class="{active: show === 'all'}">
        All
        </button>
        <button  type="button"
        @click="show = 'favorites'"
        :class="{active: show === 'favorites'}">
        Favorites
        </button>
        <!-- List Begins -->
        <p>Notes</p>
        <v-ons-list>
          <v-ons-list-item tappable modifier="nodivider" v-for="note in filteredNotes"
          href="#"
          :class="{active: activeNote === note}"
          @click="updateActiveNote(note)">
          {{note.text.trim().substring(0,15)}}
          </v-ons-list-item>
        </v-ons-list>
      </div>
      <div class="textArea">
        <textarea
        :value="activeNoteText"
        @input="editNote"
        >
        </textarea>
      </div>
    </div>
      <!-- Bottom Nav Bar -->
    <div class="navBar">
      <div class="left" @click="addNote">
        <i class="fa fa-plus"></i>
      </div>
      <div class="center" @click="toggleFavorite"
      :class="{starred: activeNote.favorite}">
        <i class="fa fa-star"></i>
      </div>
      <div class="right" @click="deleteNote">
        <i class="fa fa-minus"></i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    addNote () {
      this.$store.dispatch('addNote')
    },
    editNote (e) {
      this.$store.dispatch('editNote', e)
    },
    deleteNote () {
      this.$store.dispatch('deleteNote')
    },
    updateActiveNote (note) {
      this.$store.dispatch('updateActiveNote', note)
    },
    toggleFavorite () {
      this.$store.dispatch('toggleFavorite')
    }
  },
  computed: {
    activeNoteText () {
      return this.$store.getters.activeNote.text
    },
    filteredNotes () {
      if (this.show === 'all') {
        return this.notes
      } else if (this.show === 'favorites') {
        return this.notes.filter(note => note.favorite)
      }
    },
    activeNote () {
      return this.$store.getters.activeNote
    },
    notes () {
      return this.$store.getters.notes
    }
  },
  data () {
    return {
      show: 'all'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .container {
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    height: 100%;
  }
  .body {
    height: 90%;
    display: flex;
  }
  .navBar {
    display: flex;
    justify-content: space-around;
    align-items: center;
    height: 10%;
    background-color: lightgrey;
  }
  .sideBar {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: gold;
    width: 25%;
    height: 100%;
  }
  .list {
    width: 80%;
    overflow: scroll !important;
    overflow-y: hidden;
  }
  .list__item {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    height: 70px;
    border-bottom-width: .25px;
    border-bottom-style: solid;
  }
  .list__item-center {
    padding: 0px;
  }
  .textArea {
    display: flex;
    width: 75%;
    margin-top: 10%;
    height: 100%;
  }
  .left, .center, .right {
    width:100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  textarea {
    width: 100%;
    height: 100%;
  }
  button {
    border: 1px;
    border-style: solid;
    width: 80%;
    margin-top: 5%;
    background-color:white;
  }
  .active {
    background-color: lightgrey;
  }
  .starred {
    color: gold;
  }
  .icon {
    font-size: 30px;
  }
</style>
