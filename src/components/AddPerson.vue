<template>
    <div class="add" @click.left="showTheModal">                        
        <span class="icon-add" />
    </div>

    <teleport v-if="showModal" to='body'>
        <Modal :title="'Add a new person'" @close="hideTheModal">
            <template v-slot:modal-content>
                <form @submit.prevent="addPerson">
                    <div class="form-group">
                        <label for="firstName">First name</label>
                        <input v-model="firstName" id="firstName" name="firstName" type="text" />
                    </div>
                    <div class="form-group">
                        <label for="lastName">Last name</label>
                        <input v-model="lastName" id="lastName" name="lastName" type="text" />
                    </div>
                    <div class="form-group">
                        <label for="nickname">Nickname</label>
                        <input v-model="nickname" id="nickname" name="nickname" type="text" />
                    </div>
                    <div class="btn-group">
                        <button class="btn">Add</button>
                        <button class="btn btn-secondary" @click="hideTheModal">Cancel</button>
                    </div>
                </form>
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
  }
}
</script>

<style>
.add {
    position: relative;            
    text-align: center;
    margin: 0 0 15px;            
    padding: 0px;
    border-width: 3px;
    border-style: solid;
    border-style: dashed;  
    border-color: rgba(0, 0, 0, .3);     
    background-color: #ffffff;
    cursor: pointer;
    height: 170px;
    width: 180px;    
    -webkit-border-radius: 10px;
    -moz-border-radius: 10px;
    border-radius: 10px;
}

.add:hover {
    border-color: #000000;
}

.add .icon-add {
    background: url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAyNy43LjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjxzdmcgdmVyc2lvbj0iMS4xIiBpZD0iTGF5ZXJfMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgeD0iMHB4IiB5PSIwcHgiDQoJIHZpZXdCb3g9IjAgMCAzMCAzMCIgc3R5bGU9ImVuYWJsZS1iYWNrZ3JvdW5kOm5ldyAwIDAgMzAgMzA7IiB4bWw6c3BhY2U9InByZXNlcnZlIj4NCjxwYXRoIGQ9Ik0xNSwxLjRDNy41LDEuNCwxLjQsNy41LDEuNCwxNVM3LjUsMjguNiwxNSwyOC42YzcuNiwwLDEzLjYtNi4xLDEzLjYtMTMuNkMyOC42LDcuNSwyMi41LDEuNCwxNSwxLjR6IE0xNSwyNw0KCUM4LjQsMjcsMywyMS42LDMsMTVDMyw4LjQsOC40LDMsMTUsM2M2LjYsMCwxMiw1LjQsMTIsMTJTMjEuNiwyNywxNSwyN3ogTTE5LjgsMTVjMCwwLjUtMC4zLDAuOC0wLjgsMC44aC0zLjJWMTkNCgljMCwwLjUtMC4zLDAuOC0wLjgsMC44cy0wLjgtMC4zLTAuOC0wLjh2LTMuMkgxMWMtMC41LDAtMC44LTAuMy0wLjgtMC44czAuMy0wLjgsMC44LTAuOGgzLjJWMTFjMC0wLjUsMC4zLTAuOCwwLjgtMC44DQoJczAuOCwwLjMsMC44LDAuOHYzLjJIMTlDMTkuNSwxNC4yLDE5LjgsMTQuNSwxOS44LDE1eiIvPg0KPC9zdmc+DQo=) no-repeat center center;
    height: 100px;
    width: 100px;
    display: block;
    margin: 35px auto;
    opacity: 0.3;
}

.add:hover .icon-add {
    opacity: 1;
}
</style>