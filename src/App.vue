<template>
    <div class="container">
        <div class="title">
            {{ title }}
        </div>

        <div v-show="!showHide" class="cards">
            <div v-if="checkBtnDisabled" class="error">*Choose at least 2 persons from the total of {{ possibleHost.length }}.</div>
            <card v-for="(person, index) in possibleHost.filter(i =>  i.archive === false)"
                  :key="index"
                  :person="person"
                  @changeSelected="changeIsSelected"
                  @changeArchive="changeIsArchive" />
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

    <button @click.left="toggleModal" style="display: noone;">Add</button>

    <teleport v-if="showModal" to='#modals'>
        <Modal :title="'Add a new person'" theme="sale" @close="toggleModal">
            <template v-slot:modal-content>
                <div class="form-group">
                    <label for="firstName">First name</label>
                    <input id="firstName" name="firstName" type="text" />
                </div>
                <div class="form-group">
                    <label for="lastName">Last name</label>
                    <input id="lastName" name="lastName" type="text" />
                </div>
                <button class="btn">Save</button>
            </template>
        </Modal>
    </teleport>
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
                possibleHost: [
                    { firstName: 'Alexandru', lastName: 'Ciobotaru', isSelected: true, archive: false },
                    { firstName: 'Alex', lastName: 'Bedford', isSelected: true, archive: false },
                    { firstName: 'Andre', lastName: 'Sovde', isSelected: true, archive: false },
                    { firstName: 'Callum', lastName: 'Anderson', isSelected: true, archive: false },
                    { firstName: 'David', lastName: 'Synott', isSelected: true, archive: false },
                    { firstName: 'Matthew', lastName: 'Fosberry', isSelected: true, archive: false },
                    { firstName: 'Gavin', lastName: 'Palmer', isSelected: true, archive: false },
                    { firstName: 'Hamzah', lastName: 'Malik', isSelected: true, archive: false },
                    { firstName: 'Harrison', lastName: 'Brown', isSelected: true, archive: false },
                    { firstName: 'James', lastName: 'Darcy', isSelected: true, archive: false },
                    { firstName: 'James', lastName: 'Stratton', isSelected: true, archive: false },
                    { firstName: 'Lia', lastName: 'Gallardo', isSelected: true, archive: false },
                    { firstName: 'Nick', lastName: 'Lornie', isSelected: true, archive: false },
                    { firstName: 'Phil', lastName: 'Hoy', isSelected: true, archive: false },
                    { firstName: 'Scott', lastName: 'Milne', isSelected: true, archive: false },
                    { firstName: 'Steve', lastName: 'Odai', isSelected: true, archive: false },
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
                const name = this.selectedHost.length !== 0 ? `${randomValue.firstName} ${this.initialsLastName(randomValue.lastName)}` : 'N/A';
                const val = emoji + name + emoji;
                const value = emoji.repeat(this.selectedHost.length);
                elem.textContent = value;
                const template = this.selectedHost.length !== 0 ? `${'<span>Congratulations!</span>' + '<p>'}${val}</p>` : 'N/A';

                setTimeout(() => {
                    elem.innerHTML = template;
                }, 500);
            },
            initialsLastName(name) {
                return `${name.substring(0, 1).toUpperCase()}.`;
            },
            toggleModal() {
                this.showModal = !this.showModal
            },
            changeIsSelected(person) {
                person.isSelected = !person.isSelected;
            },
            changeIsArchive(person) {
                person.isSelected = false;
                person.archive = true;          
            }
        },
    };
</script>

<style>
    .next-host {
        position: relative;
        display: block;
        min-height: 50px;
        line-height: 2.455em;
        max-width: 100%;
        margin: 50px 0 25px;
        text-align: center;
        border: 3px solid #ff686b;
        padding: 25px 0;
        border-radius: 10px;
        font-weight: 700;
        font-size: 24px;
        color: #ff686b;
        background-color: #ffffff;
    }

        .next-host span {
            position: absolute;
            display: block;
            color: #ffffff;
            line-height: 1.5em;
            font-size: 18px;
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
</style>
