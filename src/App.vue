<template>
    <div class="title">
        {{ title }}
    </div>
    <div class="container">
        <div v-if="checkBtnDisabled 
                    && possibleHost.filter(i =>  i.archive === false).length > 1" 
            class="error">
                Choose at least 2 persons from the total of {{ possibleHost.filter(i =>  i.archive === false).length }} available.
        </div>

        <div v-show="!showHide" class="cards">
            <card v-for="(person, index) in possibleHost.filter(i =>  i.archive === false)"
                  :key="index"
                  :person="person"
                  @changeSelected="changeIsSelected"
                  @changeArchive="changeIsArchive" />

            <div class="add" @click.left="showTheModal">                        
                <span class="icon-add" />
            </div>
        </div>

        <div v-show="showHide" class="next-host" />

        <div class="btn-group">
            <button v-show="showHide"
                    class="btn"
                    @click="showHide = !showHide">
                Do a different selection
            </button>

            <button class="btn"
                    :disabled="checkBtnDisabled"
                    @click="selectPerson">
                {{ showHide ? 'Choose a different person' : 'Pick the next person' }}
            </button>
        </div>
    </div>

    <Modal :title="'Add a new person'" :openModal="showModal" @close="hideTheModal">
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
    </Modal>
</template>

<script>
    import Card from './components/Card.vue';
    import Modal from './components/Modal.vue'

    export default {
        name: 'App',
        components: {
            Card,
            Modal
        },
        data() {
            return {
                title: 'Next person for PNP',
                showHide: false,
                showModal: false,
                btnDisabled: false,
                firstName: '',
                lastName: '',
                nickname: '',
                possibleHost: [
                    { firstName: 'Alexandru', lastName: 'Ciobotaru', nickname:'Alex C', isSelected: true, archive: false },
                    { firstName: 'Alex', lastName: 'Bedford', nickname:'', isSelected: true, archive: false },
                    { firstName: 'Andre', lastName: 'Sovde', nickname:'Andre', isSelected: true, archive: false },
                    { firstName: 'Callum', lastName: 'Anderson', nickname:'Call', isSelected: true, archive: false },
                    { firstName: 'David', lastName: 'Synott', nickname:'David', isSelected: true, archive: false },
                    { firstName: 'Matthew', lastName: 'Fosberry', nickname:'Fozz', isSelected: true, archive: false },
                    { firstName: 'Gavin', lastName: 'Palmer', nickname:'Gavin', isSelected: true, archive: false },
                    { firstName: 'Hamzah', lastName: 'Malik', nickname:'Hamzah', isSelected: true, archive: false },
                    { firstName: 'Harrison', lastName: 'Brown', nickname:'Harrison', isSelected: true, archive: false },
                    { firstName: 'James', lastName: 'Darcy', nickname:'', isSelected: true, archive: false },
                    { firstName: 'James', lastName: 'Stratton', nickname:'', isSelected: true, archive: false },
                    { firstName: 'Lia', lastName: 'Gallardo', nickname:'Lia', isSelected: true, archive: false },
                    { firstName: 'Nick', lastName: 'Lornie', nickname:'Nick', isSelected: true, archive: false },
                    { firstName: 'Phil', lastName: 'Hoy', nickname:'Phil', isSelected: true, archive: false },
                    { firstName: 'Scott', lastName: 'Milne', nickname:'Scott', isSelected: true, archive: false },
                    { firstName: 'Steve', lastName: 'Odai', nickname:'Steve', isSelected: true, archive: false },
                ],
                selectedHost: [],
            };
        },
        computed: {
            checkBtnDisabled() {
                this.selectedHostFun();
                return this.selectedHost.length > 1 ? this.btnDisabled : !this.btnDisabled;
            },
        },
        methods: {
            selectedHostFun() {
                const selectedHost = [];

                for (let i = 0; i < this.possibleHost.length; i += 1) {
                    if (this.possibleHost[i].isSelected) {
                        selectedHost.push(this.possibleHost[i]);
                    }
                }

                this.selectedHost = selectedHost;
            },
            toggleIsSelected(person) {
                person.isSelected = !person.isSelected;
            },
            selectPerson() {
                this.showHide = true;
                this.selectedHostFun();

                const emoji = '⭐';
                const elem = document.querySelector('.next-host');
                const randomValue = this.selectedHost[Math.floor(Math.random() * this.selectedHost.length)];
                const personName = randomValue.nickname != '' ? randomValue.nickname : `${randomValue.firstName} ${this.initialsLastName(randomValue.lastName)}`
                const name = this.selectedHost.length !== 0 ? personName : 'N/A';
                const value = emoji.repeat(this.selectedHost.length);                
                const template = this.selectedHost.length !== 0 ? `${'<span>Congratulations!</span>' + '<p>'}${emoji + name + emoji}</p>` : 'N/A';

                elem.textContent = value;

                setTimeout(() => {
                    elem.innerHTML = template;
                }, 500);
            },
            initialsLastName(name) {
                return `${name.substring(0, 1).toUpperCase()}`;
            },
            showTheModal() {
                this.showModal = true
                document.body.classList.add('modal-open');
            },
            hideTheModal() {
                this.showModal = false;
                document.body.classList.remove('modal-open');
            },
            changeIsSelected(person) {
                person.isSelected = !person.isSelected;
            },
            changeIsArchive(person) {
                person.isSelected = false;
                person.archive = true;          
            },
            addPerson() {                
                const {firstName, lastName, nickname, isSelected = true, archive = false} = this;
                this.possibleHost.push({firstName, lastName, nickname, isSelected, archive});
                this.firstName = this.lastName = this.nickname = '';
            }
        },
    };
</script>

<style>
    .next-host {
        position: relative;
        display: block;
        min-height: 50px;
        width: 100%;
        max-width: 700px;
        margin: 40px auto 45px;
        text-align: center;
        border: 3px solid #ff686b;
        padding: 25px 0;
        border-radius: 10px;
        font-weight: 700;
        color: #ff686b;
        background-color: #ffffff;
    }

        .next-host span {
            position: absolute;
            display: block;
            color: #ffffff;
            line-height: 1.5em;
            font-weight: 500;
            width: 100%;
            background: #ff686b;
            padding: 0px 0px;
            top: 0;
            left: 0;
            right: 0;
            text-align: center;
            text-transform: uppercase;
            height: 30px;
        }

        .next-host p {
            margin: 25px 0 0;
            padding: 0;
        }

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
