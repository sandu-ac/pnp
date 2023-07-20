<template>
      <div :class="['card', {selected: person.isSelected}]"
          :key="index"
          @click.self="isPersonSelected">
          <span class="icon-x" @click="toggleArchiveModal" />
          <span class="avatar"
                @click.self="isPersonSelected" />
          <span class="name"
                @click.self="isPersonSelected"
                v-html="displayPersonName()"/>
      </div>

      <teleport v-if="showModalArchive" to='body'>
        <Modal :title="'Are you sure?'" @close="toggleArchiveModal">
            <template v-slot:modal-content>
              <p class="text-center">By clicking <b>'Yes'</b> you will archive <b v-html="displayPersonName()"/>.</p>

                <div class="btn-group">
                  <button class="btn" @click="changeArchive">Yes</button>
                  <button class="btn btn-secondary" @click="toggleArchiveModal">No</button>
                </div>
            </template>
        </Modal>
    </teleport>
</template>
  
  <script>
  import Modal from './Modal.vue'
  export default {
    name: 'CardComponent',
    components: {
        Modal
    },
    props: {
      person: {
        type: Object
      },
      index: {
        type: Number,
        default: 0
      },
    },
    emits :{
      changeSelected: null,
      changeArchive :null,
    },
    data() {
      return {
        showModalArchive: false
      }
    },
    methods: {
      isPersonSelected () {
        this.$emit('changeSelected', this.person);
      },
      changeArchive () {
        this.showModalArchive = false;
        this.$emit('changeArchive', this.person);
      },
      initialsLastName(name) {
          return `${name.substring(0, 1).toUpperCase()}`;
      },
      toggleArchiveModal() {
          this.showModalArchive = !this.showModalArchive
      },
      displayPersonName()
      {
        const personName = this.person.nickname != '' ? `${this.person.nickname}` : `${this.person.firstName} ${this.initialsLastName(this.person.lastName)}`
        return personName;
      }
    }
  }
  </script>
  
  <style>    
    .cards {
        position: relative;
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
        margin: 0px auto;
        padding: 30px 0 30px;
        width: calc(200px * 6);
    }

    .card {
        margin: 0 0 15px;
        text-align: center;
        padding: 0px;
        border-width: 3px;
        border-style: solid;
        border-color: rgba(0, 0, 0, .3);
        cursor: pointer;
        height: 170px;
        position: relative;
        width: 180px;
        background-color: #ffffff;
        -webkit-border-radius: 10px;
        -moz-border-radius: 10px;
        border-radius: 10px;
    }

    .card:hover,
    .card.selected:hover {
        box-shadow: 0 10px 10px rgba(0,0,0,0.05), 0 5px 5px rgba(0,0,0,0.05);
    }

    .card.selected {
        border-color: #ff686b;
    }

    .card:before,
    .card.selected:before {
        position: absolute;
        text-align: center;
        margin: 0 auto;
        padding: 0 10px;
        width: 100px;
        height: 20px;        
        left: 0;
        right: 0;
        bottom: 0;        
        font-size: 12px;
        line-height: 2em;
        font-weight: 500;
        text-transform: uppercase;
        border-width: 3px 3px 0px 3px;
        border-style: solid;
        -webkit-border-radius: 10px 10px 0 0;
        -moz-border-radius: 10px 10px 0 0;
        border-radius: 10px 10px 0 0;
    }

    .card:before {
        content: 'Unselected';
        color: #000000;
        border-color: rgba(0, 0, 0, .7);
        background-color: transparent;
    }

    .card.selected:before {
        content: 'Selected';
        color: #ffffff;
        border-bottom-width: 0px;
        border-color: #ff686b;
        background-color: #ff686b;
    }

    .card .avatar {
        background: url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAyNy43LjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjxzdmcgdmVyc2lvbj0iMS4xIiBpZD0iTGF5ZXJfMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgeD0iMHB4IiB5PSIwcHgiDQoJIHZpZXdCb3g9IjAgMCAzMCAzMCIgc3R5bGU9ImVuYWJsZS1iYWNrZ3JvdW5kOm5ldyAwIDAgMzAgMzA7IiB4bWw6c3BhY2U9InByZXNlcnZlIj4NCjxwYXRoIGQ9Ik0xNSwxLjRDNy41LDEuNCwxLjQsNy41LDEuNCwxNWMwLDcuNSw2LjEsMTMuNiwxMy42LDEzLjZTMjguNiwyMi41LDI4LjYsMTVDMjguNiw3LjUsMjIuNSwxLjQsMTUsMS40eiBNMTUsMjcuMQ0KCUM4LjMsMjcuMSwyLjksMjEuNywyLjksMTVDMi45LDguMyw4LjMsMi45LDE1LDIuOVMyNy4xLDguMywyNy4xLDE1QzI3LjEsMjEuNywyMS43LDI3LjEsMTUsMjcuMXogTTE1LDEzLjVjMi4xLTAuMSwzLjctMS44LDMuNy0zLjkNCgljMC4xLTIuMS0xLjYtMy44LTMuNy0zLjljLTIuMSwwLjEtMy43LDEuOC0zLjcsMy45QzExLjMsMTEuOCwxMi45LDEzLjUsMTUsMTMuNXogTTEyLjgsOS42QzEyLjgsOSwxMyw4LjQsMTMuNCw4DQoJYzAuNC0wLjQsMS0wLjcsMS42LTAuN2MxLjMsMC4xLDIuMiwxLjEsMi4yLDIuNGwwLDBsMCwwYzAsMC42LTAuMiwxLjItMC42LDEuN2MtMC40LDAuNC0xLDAuNy0xLjYsMC43Yy0wLjYsMC0xLjItMC4zLTEuNi0wLjcNCglDMTMsMTAuOSwxMi44LDEwLjMsMTIuOCw5LjZMMTIuOCw5LjZMMTIuOCw5LjZ6IE0yMi44LDIyLjNjMCwwLjQtMC4zLDAuOC0wLjgsMC44cy0wLjgtMC4zLTAuOC0wLjhjMC0zLjQtMi44LTYuMi02LjItNi4yDQoJcy02LjIsMi44LTYuMiw2LjJDOC44LDIyLjcsOC40LDIzLDgsMjNzLTAuOC0wLjMtMC44LTAuOGMwLTQuMywzLjUtNy44LDcuOC03LjhTMjIuOCwxOCwyMi44LDIyLjN6Ii8+DQo8L3N2Zz4NCg==) no-repeat center center;
        height: 70px;
        display: block;
        margin: 25px 25px 10px;
    }

    .card .icon-x {
      cursor: pointer;
      background: url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAyNy43LjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjxzdmcgdmVyc2lvbj0iMS4xIiBpZD0iTGF5ZXJfMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgeD0iMHB4IiB5PSIwcHgiDQoJIHZpZXdCb3g9IjAgMCAzMCAzMCIgc3R5bGU9ImVuYWJsZS1iYWNrZ3JvdW5kOm5ldyAwIDAgMzAgMzA7IiB4bWw6c3BhY2U9InByZXNlcnZlIj4NCjxwYXRoIGQ9Ik0yMy41LDIyLjVjMC4zLDAuMywwLjMsMC44LDAsMS4xYzAsMC4yLTAuMywwLjItMC41LDAuMmMtMC4yLDAtMC41LDAtMC42LTAuMkwxNSwxNi4xbC03LjUsNy41YzAsMC4yLTAuMywwLjItMC41LDAuMg0KCWMtMC4yLDAtMC41LDAtMC42LTAuMmMtMC4zLTAuMy0wLjMtMC44LDAtMS4xbDcuNS03LjVMNi41LDcuNmMtMC4zLTAuMy0wLjMtMC44LDAtMS4xczAuOC0wLjMsMS4xLDBsNy41LDcuNWw3LjUtNy41DQoJYzAuMy0wLjMsMC44LTAuMywxLjEsMHMwLjMsMC44LDAsMS4xTDE2LjEsMTVMMjMuNSwyMi41eiIvPg0KPC9zdmc+DQo=) no-repeat center center;
      height: 20px;
      width: 20px;
      position: absolute;
      top: 5px;
      right: 5px;
      display: block;
      margin: 0px;
      padding: 5px;
      opacity: 0.3;
      background-color: transparent;
      display: none;
      -webkit-border-radius: 50%;
      -moz-border-radius: 50%;
      border-radius: 50%;
    }

    .card .icon-x:hover {
      background: url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAyNy43LjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjxzdmcgdmVyc2lvbj0iMS4xIiBpZD0iTGF5ZXJfMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgeD0iMHB4IiB5PSIwcHgiDQoJIHZpZXdCb3g9IjAgMCAzMCAzMCIgc3R5bGU9ImVuYWJsZS1iYWNrZ3JvdW5kOm5ldyAwIDAgMzAgMzA7IiB4bWw6c3BhY2U9InByZXNlcnZlIj4NCjxzdHlsZSB0eXBlPSJ0ZXh0L2NzcyI+DQoJLnN0MHtmaWxsOiNGRkZGRkY7fQ0KPC9zdHlsZT4NCjxwYXRoIGNsYXNzPSJzdDAiIGQ9Ik0yMy41LDIyLjVjMC4zLDAuMywwLjMsMC44LDAsMS4xYzAsMC4yLTAuMywwLjItMC41LDAuMnMtMC41LDAtMC42LTAuMkwxNSwxNi4xbC03LjUsNy41DQoJYzAsMC4yLTAuMywwLjItMC41LDAuMnMtMC41LDAtMC42LTAuMmMtMC4zLTAuMy0wLjMtMC44LDAtMS4xbDcuNS03LjVMNi41LDcuNmMtMC4zLTAuMy0wLjMtMC44LDAtMS4xczAuOC0wLjMsMS4xLDBsNy41LDcuNQ0KCWw3LjUtNy41YzAuMy0wLjMsMC44LTAuMywxLjEsMHMwLjMsMC44LDAsMS4xTDE2LjEsMTVMMjMuNSwyMi41eiIvPg0KPC9zdmc+DQo=) no-repeat center center;
      background-color: #ff686b;
      opacity: 1;
    }

    .card:hover .icon-x {
      display: block;
    }

    .card .name {
        display: block;
        font-weight: 700;
        margin: 0;
        padding: 0 10px;
    }

    .card:before,
    .card .avatar,
    .card .name {
        opacity: 0.3;
    }

    .card.selected:before,
    .card.selected .avatar,
    .card.selected .name {
        opacity: 1;
    }

    .card.selected .avatar,
    .card.selected .name {
        color: #ff686b;
    }
  </style>
  