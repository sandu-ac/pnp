<template>
    <div class="backdrop" @click.self.prevent="closeModal">
        <div class="modal">
            <span class="icon-x" @click.self.prevent="closeModal"></span>
            <h1>{{ title }}</h1>
            <slot name="modal-content"></slot>
        </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ModalComponent',
    props: {
      title: {
        type: String,
        default: ''
      },
    },
    methods: {
      closeModal () {
        this.$emit('close')
      },
      initialsLastName(name) {
            return `${name.substring(0, 1).toUpperCase()}.`;
        },
    }
  }
  </script>
  
  <style>
    .backdrop {
      top: 0;
      position: fixed;
      background-color: rgba(0, 0, 0, 0.5);
      width: 100%;
      height: 100%;
    }
    .modal{
      width: 500px;
      position: relative;
      padding: 35px;
      margin: 100px auto;
      background-color: #ffffff;
      border-radius: 10px;
      box-shadow: 0 10px 10px rgba(0,0,0,0.05), 0 5px 5px rgba(0,0,0,0.05);
    }
    .modal h1 {
        color: #ff686b;
        border: none;
        text-align: center;
        font-size: 28px;
        margin: 0 0 25px;
        padding: 0;
    }
    .modal .btn {
        display: block;
        text-align: center;
        margin: 0 auto;
    }
    .modal .btn-group {
      margin-top: 25px;;
    }
    .modal .btn-group .btn {
      display: inline-block;
      margin: 0 15px;
    }
    .modal .icon-x {
      cursor: pointer;
      background: url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAyNy43LjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjxzdmcgdmVyc2lvbj0iMS4xIiBpZD0iTGF5ZXJfMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgeD0iMHB4IiB5PSIwcHgiDQoJIHZpZXdCb3g9IjAgMCAzMCAzMCIgc3R5bGU9ImVuYWJsZS1iYWNrZ3JvdW5kOm5ldyAwIDAgMzAgMzA7IiB4bWw6c3BhY2U9InByZXNlcnZlIj4NCjxwYXRoIGQ9Ik0yMy41LDIyLjVjMC4zLDAuMywwLjMsMC44LDAsMS4xYzAsMC4yLTAuMywwLjItMC41LDAuMmMtMC4yLDAtMC41LDAtMC42LTAuMkwxNSwxNi4xbC03LjUsNy41YzAsMC4yLTAuMywwLjItMC41LDAuMg0KCWMtMC4yLDAtMC41LDAtMC42LTAuMmMtMC4zLTAuMy0wLjMtMC44LDAtMS4xbDcuNS03LjVMNi41LDcuNmMtMC4zLTAuMy0wLjMtMC44LDAtMS4xczAuOC0wLjMsMS4xLDBsNy41LDcuNWw3LjUtNy41DQoJYzAuMy0wLjMsMC44LTAuMywxLjEsMHMwLjMsMC44LDAsMS4xTDE2LjEsMTVMMjMuNSwyMi41eiIvPg0KPC9zdmc+DQo=) no-repeat center center;
      height: 20px;
      width: 20px;
      position: absolute;
      top: 0;
      right: 0;
      display: block;
      margin: 10px 10px;
      padding: 5px;
      opacity: 0.3;
      background-color: transparent;
      -webkit-border-radius: 50%;
      -moz-border-radius: 50%;
      border-radius: 50%;
    }

    .modal .icon-x:hover {
      background: url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAyNy43LjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjxzdmcgdmVyc2lvbj0iMS4xIiBpZD0iTGF5ZXJfMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgeD0iMHB4IiB5PSIwcHgiDQoJIHZpZXdCb3g9IjAgMCAzMCAzMCIgc3R5bGU9ImVuYWJsZS1iYWNrZ3JvdW5kOm5ldyAwIDAgMzAgMzA7IiB4bWw6c3BhY2U9InByZXNlcnZlIj4NCjxzdHlsZSB0eXBlPSJ0ZXh0L2NzcyI+DQoJLnN0MHtmaWxsOiNGRkZGRkY7fQ0KPC9zdHlsZT4NCjxwYXRoIGNsYXNzPSJzdDAiIGQ9Ik0yMy41LDIyLjVjMC4zLDAuMywwLjMsMC44LDAsMS4xYzAsMC4yLTAuMywwLjItMC41LDAuMnMtMC41LDAtMC42LTAuMkwxNSwxNi4xbC03LjUsNy41DQoJYzAsMC4yLTAuMywwLjItMC41LDAuMnMtMC41LDAtMC42LTAuMmMtMC4zLTAuMy0wLjMtMC44LDAtMS4xbDcuNS03LjVMNi41LDcuNmMtMC4zLTAuMy0wLjMtMC44LDAtMS4xczAuOC0wLjMsMS4xLDBsNy41LDcuNQ0KCWw3LjUtNy41YzAuMy0wLjMsMC44LTAuMywxLjEsMHMwLjMsMC44LDAsMS4xTDE2LjEsMTVMMjMuNSwyMi41eiIvPg0KPC9zdmc+DQo=) no-repeat center center;
      background-color: rgba(0, 0, 0, .2);
      background-color: #ff686b;
      opacity: 1;
    }
  </style>
  