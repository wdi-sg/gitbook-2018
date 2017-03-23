# jQuery vs. Vue  

We took a simple chat app starter kit written with HTML and jQuery, and replaced all jQuery methods with Vue.js. This section will show you a side-by-side comparison of some parts of the code. It will help you understand more clearly how Vue works.

* [Chat App Starter Kit with jQuery](https://github.com/jeremiahalex/simple-chat-app)
* [Chat App with jQuery, converted to Vue](http://vue-group-chat.herokuapp.com/index-2.html)

## HTML Elements vs. Components

'JoinForm' element on HTML / jQuery:

```
<section id="join" class="well hidden">
  <form id="JoinForm" class="form-inline text-right">
      <fieldset>
        <input type="text" class="form-control " placeholder="Your name" autocomplete="off" required autofocus />
        <button id="sendJoin" class="btn btn-success" disabled>Join</button>
      </fieldset>
  </form>
</section>

<script>
  $('#JoinForm input').on('input', function () {
    $('#sendJoin').prop('disabled', $(this).val().length === 0)
  })

  $('#JoinForm').submit(function (event) {
    user.name = $('#JoinForm input').val()
    if (user.name.length === 0) return false

    console.log('Joining chat with name: ', user.name)
    socket.emit('join', user)
    $('#sendJoin').focus()

    $('section#join').addClass('hidden')
    return false
  })
</script>
```

Same 'JoinForm' element, converted to a Vue component:

```
Vue.component('join-form', {
  props: ['status'],

  template: `
  <section id="join" v-bind:hidden="isNameRegistered">
    <form id="JoinForm" class="text-right">

        <fieldset>
          <div class="input-group input-group-lg">
          <input type="text" class="form-control" v-model="inputMessage" placeholder="Enter your name" autocomplete="off" required autofocus />
          <span class="input-group-btn">
            <button id="sendJoin" class="btn btn-success" v-bind:disabled="!isThereAName" v-on:click.prevent="registerUserName">
            <span class="glyphicon glyphicon-user"></span>
            </button>
          </span>
          </div>
        </fieldset>

    </form>
  </section>
  `,
  data: function() {
    return {
      inputMessage: '',
      isThereAName: false,
      isNameRegistered: false
    }
  },
  watch: {
    inputMessage: function( nameField ) {
      if (nameField === '') this.isThereAName = false
      if (nameField !== '') this.isThereAName = true
    }
  },
  methods: {
    registerUserName: function() {
      console.log('running registerUserName')
      user.name = this.inputMessage
      if (user.name.length === 0) return false

      console.log('Joining chat with name: ', user.name)
      socket.emit('join', user)

      // hide join form upon successful username
      this.isNameRegistered = true
    }
  }
})
```

## Adding Message Elements

```
<script>
$('#MessageForm').submit(function (event) {
    var msg = $('#MessageForm input').val()
    if (msg.length === 0) return false

    $('#MessageForm input').val('')
    console.log('Sending message: ', msg)
    socket.emit('chat', msg)

    $('#messages').prepend($('<div class="alert alert-info text-right">').text(msg))
    })
})

socket.on('chat', function (msg) {
  console.log('Received message: ', msg)
  $('#messages').prepend($('<div class="alert alert-success">').html('<strong>' + msg.user.name + ':</strong> ' + msg.message))
})
</script>

```

'MessageForm', Converted to Vue component:

```
Vue.component('chat-message', {
  props: ['message'],
  template: `
  <div class="text-center grey" v-if="message.type=='info'">
    <strong> {{ message.content }} </strong>
  </div>
  <div class="alert alert-info text-right" v-else-if="message.type=='success'">
    <b><small>You</small></b><br>
    {{ message.content }}
  </div>
  <div class="alert alert-success" v-else-if="message.type=='alert'">
    <b><small>{{ message.user }}</small></b><br>
    {{ message.content }}
  </div>
  `
})

Vue.component('message-board', {
  props: ['config', 'user'],
  template: `
  <main class="panel panel-success" v-show="user.name !== 'anon' ">
  <div class="panel-heading">
    <form id="MessageForm" class="text-right">

      <fieldset>
        <div class="input-group input-group-lg">
        <input type="text" class="form-control" v-model="inputSentence" placeholder="Type your message" autocomplete="off" required autofocus />
        <span class="input-group-btn">
          <button id="sendMessage" class="btn btn-success" v-bind:disabled="!isThereSentence" v-on:click.prevent="postChatMessage">
          <span class="glyphicon glyphicon-send"></span>
          </button>
        </span>
        </div>
      </fieldset>

    </form>
  </div>

  <section class="panel-body">
    <div class="text-center">
      <small id="connected">
        {{ config.connected }}
      </small>
    </div>
    <hr>

    <div id="messages" v-for="message in config.messages">
      <chat-message v-bind:message="message"></chat-message>
    </div>
  </section>

  </main>
  `,
  data: function(){
    return {
      inputSentence: '',
      isThereSentence: false,
      isNameRegistered: false
    }
  },
  watch: {
    inputSentence: function(textField) {
      if (textField === '') this.isThereSentence = false
      if (textField !== '') this.isThereSentence = true
    }
  },
  methods: {
    postChatMessage: function() {
      console.log('Sending message: ' + this.inputSentence)
      var msg = this.inputSentence
      if (msg.length === 0) return false
      config.messages.unshift(
        { type: 'success', content: msg }
      )
      console.log('Messages Array: ', config.messages)
      socket.emit('chat', msg)

      this.inputSentence = ''
    }
  }
})
```
