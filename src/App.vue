<template>
    <div class="container">
        <div class="title">
            {{ title }}
        </div>

        <div v-if="!showHide" class="cards">
            <div v-if="checkBtnDisabled" class="error">*Choose at least 2 persons from the total of {{ possibleHost.length }}.</div>

            <div
                v-for="person in possibleHost"
                :class="['card', {selected: person.isSelected}]"
                :key="person.index"
                @click="toggleIsSelected(person)">
                <span class="avatar" />
                <span class="name">{{ person.firstName }} {{ initialsLastName(person.lastName) }}</span>
            </div>
        </div>

        <div v-show="showHide" class="next-host" />

        <div class="btn-group">
            <button
                v-show="showHide"
                class="btn"
                @click="showHide = !showHide">
                Do a different selection
            </button>

            <button
                class="btn"
                :disabled="checkBtnDisabled"
                @click="selectPerson">
                {{ showHide ? 'Choose a different person' : 'Pick the next person' }}
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'App',
    components: {},
    data() {
        return {
            title: 'Next person for PNP',
            showHide: false,
            btnDisabled: false,
            possibleHost: [
                { firstName: 'Alexandru', lastName: 'Ciobotaru', isSelected: true },
                { firstName: 'Alex', lastName: 'Bedford', isSelected: true },
                { firstName: 'Andre', lastName: 'Sovde', isSelected: true },
                { firstName: 'Callum', lastName: 'Anderson', isSelected: true },
                { firstName: 'David', lastName: 'Synott', isSelected: true },
                { firstName: 'Matthew', lastName: 'Fosberry', isSelected: true },
                { firstName: 'Gavin', lastName: 'Palmer', isSelected: true },
                { firstName: 'Hamzah', lastName: 'Malik', isSelected: true },
                { firstName: 'Harrison', lastName: 'Brown', isSelected: true },
                { firstName: 'James', lastName: 'Darcy', isSelected: true },
                { firstName: 'James', lastName: 'Stratton', isSelected: true },
                { firstName: 'Lia', lastName: 'Gallardo', isSelected: true },
                { firstName: 'Nick', lastName: 'Lornie', isSelected: true },
                { firstName: 'Phil', lastName: 'Hoy', isSelected: true },
                { firstName: 'Scott', lastName: 'Milne', isSelected: true },
                { firstName: 'Steve', lastName: 'Odai', isSelected: true },
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
    },
};
</script>

<style>
    body {
        font-family: 'Open Sans', 'Helvetica Neue', 'Arial', sans-serif;
        font-size: 20px;
        letter-spacing: 1.5px;
        background-color: #84dcc6;
    }

    .container {
        display: block;
        width: 1200px;
        margin: 50px auto;
        padding: 10px;
    }

    .title {
        color: #ffffff;
        font-size: 50px;
        font-weight: 700;
        text-shadow: 0 1px 0 #ccc, 0 2px 0 #c9c9c9, 0 3px 0 #bbb, 0 4px 0 #b9b9b9, 0 5px 0 #aaa, 0 6px 1px rgba(0, 0, 0, 0.1), 0 0 5px rgba(0, 0, 0, 0.1), 0 1px 3px rgba(0, 0, 0, 0.3), 0 3px 5px rgba(0, 0, 0, 0.2), 0 5px 10px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.2), 0 20px 20px rgba(0, 0, 0, 0.15);
        text-align: center;
        margin-bottom: 15px;
    }

    .error {
        position: absolute;
        color: #ffffff;
        background-color: #f54337;
        margin: 0 auto 25px;
        top: 0;
        left: 0;
        right: 0;
        font-weight: 700;
        padding: 5px 10px;
        text-align: center;
        max-width: 550px;
    }

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

    .btn-group {
        display: block;
        margin: 0 auto;
        text-align: center;
    }

    .btn {
        display: inline-block;
        margin: 0 15px;
        ;
        font-size: 20px;
        letter-spacing: 1.5px;
        text-align: center;
        padding: 17px 40px;
        border-radius: 10px;
        border: 0;
        background-color: #ffa69e;
        transition: all 0.3s ease;
        box-shadow: #ff686b 0px 10px 0px 0px;
        color: #ffffff;
        font-weight: 700;
        cursor: pointer;
    }

        .btn:hover {
            box-shadow: #ff686b 0px 7px 0px 0px;
        }

        .btn:active {
            background-color: #ffa69e;
            /*50, 168, 80*/
            box-shadow: #ff686b 0px 0px 0px 0px;
            transform: translateY(5px);
            transition: 200ms;
        }

        .btn:disabled,
        .btn[disabled] {
            cursor: not-allowed;
            background-color: #cccccc;
            color: #666666;
            box-shadow: #666666 0px 7px 0px 0px;
            opacity: 0.5;
        }

    .cards {
        position: relative;
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
        margin: 0px auto;
        padding: 50px 0 0;
        width: calc(200px * 6);
    }

    .card {
        margin: 0 0 15px;
        text-align: center;
        padding: 0px;
        border-width: 3px;
        border-style: solid;
        border-color: transparent;
        cursor: pointer;
        height: 160px;
        position: relative;
        width: 180px;
        background-color: #ffffff;
        font-size: 16px;
        -webkit-border-radius: 10px;
        -moz-border-radius: 10px;
        border-radius: 10px;
    }

        .card:hover,
        .selected:hover {
            box-shadow: 0 10px 10px rgba(0,0,0,0.05), 0 5px 5px rgba(0,0,0,0.05);
        }

    .selected {
        border-color: #ff686b;
    }

        .card:before,
        .selected:before {
            position: absolute;
            text-align: center;
            margin: 0 auto;
            width: 100px;
            height: 10px;
            height: 20px;
            padding: 0 5px;
            left: 0;
            right: 0;
            font-size: 12px;
            font-weight: 700;
            text-transform: uppercase;
        }

    .card:before {
        content: 'Unselected';
        color: #000000;
        background-color: transparent;
    }

    .selected:before {
        content: 'Selected';
        color: #ffffff;
        background-color: #ff686b;
    }

    .avatar {
        background: url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAyNy43LjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjxzdmcgdmVyc2lvbj0iMS4xIiBpZD0iTGF5ZXJfMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgeD0iMHB4IiB5PSIwcHgiDQoJIHZpZXdCb3g9IjAgMCA1MCA1MCIgc3R5bGU9ImVuYWJsZS1iYWNrZ3JvdW5kOm5ldyAwIDAgNTAgNTA7IiB4bWw6c3BhY2U9InByZXNlcnZlIj4NCjxwYXRoIGQ9Ik0yNSwxQzExLjcsMSwxLDExLjcsMSwyNXMxMC43LDI0LDI0LDI0czI0LTEwLjcsMjQtMjRTMzguMywxLDI1LDF6IE0yNSw0N0MxMi44LDQ3LDMsMzcuMiwzLDI1UzEyLjgsMywyNSwzczIyLDkuOCwyMiwyMg0KCVMzNy4yLDQ3LDI1LDQ3eiBNMjUsMjUuNGMtNy4yLDAtMTMsNS44LTEzLDEzYzAsMC42LDAuNCwxLDEsMXMxLTAuNCwxLTFjMC02LjEsNC45LTExLDExLTExczExLDQuOSwxMSwxMWwwLDBjMCwwLjYsMC40LDEsMSwxDQoJczEtMC40LDEtMUMzOCwzMS4yLDMyLjIsMjUuNCwyNSwyNS40eiBNMjUsMjMuN2MzLjktMC4xLDYuOS0zLjMsNi44LTcuMmMwLDAsMCwwLDAsMGMwLjEtMy45LTIuOS03LjEtNi44LTcuMmMwLDAsMCwwLDAsMA0KCWMtMy45LDAuMS02LjksMy4zLTYuOCw3LjJjMCwwLDAsMCwwLDBDMTguMSwyMC40LDIxLjEsMjMuNiwyNSwyMy43QzI1LDIzLjcsMjUsMjMuNywyNSwyMy43eiBNMjUsMTEuM2MyLjgsMC4xLDQuOSwyLjQsNC44LDUuMg0KCWMwLDAsMCwwLDAsMGMwLjEsMi44LTIsNS4xLTQuOCw1LjJjMCwwLDAsMCwwLDBjLTIuOC0wLjEtNC45LTIuNC00LjgtNS4yYzAsMCwwLDAsMCwwQzIwLjEsMTMuOCwyMi4yLDExLjQsMjUsMTEuMw0KCUMyNSwxMS4zLDI1LDExLjMsMjUsMTEuM3oiLz4NCjwvc3ZnPg0K) no-repeat center center;
        height: 80px;
        display: block;
        margin: 35px 25px 10px;
    }

    .name {
        display: block;
        font-weight: 700;
        margin: 0;
        padding: 0 10px;
        ;
    }

    .card:before,
    .avatar,
    .name {
        opacity: 0.3;
    }

    .selected:before,
    .selected .avatar,
    .selected .name {
        opacity: 1;
    }

    .selected .avatar,
    .selected .name {
        color: #ff686b;
    }
</style>
