<!-- HTML and CSS obtained from: https://bootsnipp.com/snippets/1ea0N -->
<template>
  <div class="home">
    <div class="container">
    <h3 class=" text-center">Messaging</h3>
    <div class="messaging">
          <div class="inbox_msg">
            <div class="inbox_people">
              <div class="headind_srch">
                <div class="recent_heading">
                  <h4>Recent</h4>
                </div>
                <div class="srch_bar">
                  <div class="stylish-input-group">
                    <input type="text" class="search-bar"  placeholder="Search" >
                    <span class="input-group-addon">
                    <button type="button"> <i class="fa fa-search" aria-hidden="true"></i> </button>
                    </span> </div>
                </div>
              </div>
              <div class="inbox_chat">
                <div class="chat_list active_chat">
                  <div class="chat_people">
                    <div class="chat_img"> <img src="https://ptetutorials.com/images/user-profile.png" alt="sunil"> </div>
                    <div class="chat_ib">
                      <h5>Sunil Rajput <span class="chat_date">Dec 25</span></h5>
                      <p>Test, which is a new approach to have all solutions
                        astrology under one roof.</p>
                    </div>
                  </div>
                </div>
                <div class="chat_list">
                  <div class="chat_people">
                    <div class="chat_img"> <img src="https://ptetutorials.com/images/user-profile.png" alt="sunil"> </div>
                    <div class="chat_ib">
                      <h5>Sunil Rajput <span class="chat_date">Dec 25</span></h5>
                      <p>Test, which is a new approach to have all solutions
                        astrology under one roof.</p>
                    </div>
                  </div>
                </div>
                <div class="chat_list">
                  <div class="chat_people">
                    <div class="chat_img"> <img src="https://ptetutorials.com/images/user-profile.png" alt="sunil"> </div>
                    <div class="chat_ib">
                      <h5>Sunil Rajput <span class="chat_date">Dec 25</span></h5>
                      <p>Test, which is a new approach to have all solutions
                        astrology under one roof.</p>
                    </div>
                  </div>
                </div>
                <div class="chat_list">
                  <div class="chat_people">
                    <div class="chat_img"> <img src="https://ptetutorials.com/images/user-profile.png" alt="sunil"> </div>
                    <div class="chat_ib">
                      <h5>Sunil Rajput <span class="chat_date">Dec 25</span></h5>
                      <p>Test, which is a new approach to have all solutions
                        astrology under one roof.</p>
                    </div>
                  </div>
                </div>
                <div class="chat_list">
                  <div class="chat_people">
                    <div class="chat_img"> <img src="https://ptetutorials.com/images/user-profile.png" alt="sunil"> </div>
                    <div class="chat_ib">
                      <h5>Sunil Rajput <span class="chat_date">Dec 25</span></h5>
                      <p>Test, which is a new approach to have all solutions
                        astrology under one roof.</p>
                    </div>
                  </div>
                </div>
                <div class="chat_list">
                  <div class="chat_people">
                    <div class="chat_img"> <img src="https://ptetutorials.com/images/user-profile.png" alt="sunil"> </div>
                    <div class="chat_ib">
                      <h5>Sunil Rajput <span class="chat_date">Dec 25</span></h5>
                      <p>Test, which is a new approach to have all solutions
                        astrology under one roof.</p>
                    </div>
                  </div>
                </div>
                <div class="chat_list">
                  <div class="chat_people">
                    <div class="chat_img"> <img src="https://ptetutorials.com/images/user-profile.png" alt="sunil"> </div>
                    <div class="chat_ib">
                      <h5>Sunil Rajput <span class="chat_date">Dec 25</span></h5>
                      <p>Test, which is a new approach to have all solutions
                        astrology under one roof.</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="mesgs">
              <div class="msg_history">
                <!-- TODO: Modify and verify satisfactory layout of sent and received messages -->
                <!--       - Currently, if the same user sends multiple messages, the messages
                             appears from right to left
                           - Should include a timestamp along with message.author -->
                <div v-for="message in messages" class="incoming_msg">
                  <div :class="[message.author===authUser.displayName ? 'sent_msg':'received_msg']">
                    <div class="received_withd_msg">
                      <p>{{ message.message }}</p>
                      <span class="time_date"> {{ message.author }} </span></div>
                  </div>
                </div>
              </div>
              <div class="type_msg">
                <div class="input_msg_write">
                  <input @keyup.enter="saveMessage" v-model="message" type="text" class="write_msg" placeholder="Type a message" />
                  <button class="msg_send_btn" type="button"><i class="fa fa-paper-plane-o" aria-hidden="true"></i></button>
                </div>
              </div>
            </div>
          </div>

          <p class="text-center top_spac"> Design by <a target="_blank" href="#">Sunil Rajput</a></p>
        </div></div>
  </div>
</template>

<script>
import firebase from "firebase"

export default {
  name: "PrivateChat",

  data() {
    return {
      message: null,  // Specified by v-mode1 tag attribute
      messages: [],
      authUser:  {}
    }
  },

  methods: {
    scrollToBottom() {
      // Call this method whenever you need the scrollbar
      // for the message window to scroll down automatically
      let box = document.querySelector(".msg_history");
      box.scrollTop = box.scrollHeight;
    },

    saveMessage() {
      // Save to Firestore
      db.collection("chat").add({
        message: this.message,
        author: this.authUser.displayName,
        createdAt: new Date(),
      }).then(() => {
        this.scrollToBottom();
      }).catch((error) => {
        console.log("Error adding document: ", error);
      });

      // Clear input field
      this.message = null;
    },

    fetchMessages() {
      // onSnapshot() makes real-time display of messages possible. This
      // method call seems to contain the call to get(), which gets the
      // message data from our "chat" DB in Firebase.
      db.collection("chat").orderBy("createdAt").onSnapshot((querySnapshot) => {
        let allMessages = [];
        querySnapshot.forEach((doc) => {
          allMessages.push(doc.data());
        });
        // NOTE: Generally speaking, it's probably not a good idea
        //       to push ALL the messages into the message window,
        //       for it could be quite an expensive operation.

        this.messages = allMessages;

        setTimeout(() => {
          this.scrollToBottom();
        }, 1000);
        // Give messages 1 second to render before calling
        // scrollToBottom. scrollToBottom won't work if
        // messages haven't already been rendered.
        // - This probably isn't the most robust
        //   way of doing things...
      });
    }
  },

  created() {
    firebase.auth().onAuthStateChanged((user) => {
      if (user) {
        this.authUser = user;
      } else {
        this.authUser = {};
      }
    });
    this.fetchMessages();
  },

  beforeRouteEnter(to, from, next) {
    // vm is like "this"
    next((vm) => {
      firebase.auth().onAuthStateChanged((user) => {
        if (user) {
          next();
        } else {
          vm.$router.push("/login");
        }
      });
    });
  }
}
</script>


<style scoped="">
.container{max-width:1170px; margin:auto;}
img{ max-width:100%;}
.inbox_people {
  background: #f8f8f8 none repeat scroll 0 0;
  float: left;
  overflow: hidden;
  width: 40%; border-right:1px solid #c4c4c4;
}
.inbox_msg {
  border: 1px solid #c4c4c4;
  clear: both;
  overflow: hidden;
}
.top_spac{ margin: 20px 0 0;}


.recent_heading {float: left; width:40%;}
.srch_bar {
  display: inline-block;
  text-align: right;
  width: 60%; padding:
}
.headind_srch{ padding:10px 29px 10px 20px; overflow:hidden; border-bottom:1px solid #c4c4c4;}

.recent_heading h4 {
  color: #05728f;
  font-size: 21px;
  margin: auto;
}
.srch_bar input{ border:1px solid #cdcdcd; border-width:0 0 1px 0; width:80%; padding:2px 0 4px 6px; background:none;}
.srch_bar .input-group-addon button {
  background: rgba(0, 0, 0, 0) none repeat scroll 0 0;
  border: medium none;
  padding: 0;
  color: #707070;
  font-size: 18px;
}
.srch_bar .input-group-addon { margin: 0 0 0 -27px;}

.chat_ib h5{ font-size:15px; color:#464646; margin:0 0 8px 0;}
.chat_ib h5 span{ font-size:13px; float:right;}
.chat_ib p{ font-size:14px; color:#989898; margin:auto}
.chat_img {
  float: left;
  width: 11%;
}
.chat_ib {
  float: left;
  padding: 0 0 0 15px;
  width: 88%;
}

.chat_people{ overflow:hidden; clear:both;}
.chat_list {
  border-bottom: 1px solid #c4c4c4;
  margin: 0;
  padding: 18px 16px 10px;
}
.inbox_chat { height: 550px; overflow-y: scroll;}

.active_chat{ background:#ebebeb;}

.incoming_msg_img {
  display: inline-block;
  width: 6%;
}
.received_msg {
  display: inline-block;
  padding: 0 0 0 10px;
  vertical-align: top;
  width: 92%;
 }
 .received_withd_msg p {
  background: #ebebeb none repeat scroll 0 0;
  border-radius: 3px;
  color: #646464;
  font-size: 14px;
  margin: 0;
  padding: 5px 10px 5px 12px;
  width: 100%;
}
.time_date {
  color: #747474;
  display: block;
  font-size: 12px;
  margin: 8px 0 0;
}
.received_withd_msg { width: 57%;}
.mesgs {
  float: left;
  padding: 30px 15px 0 25px;
  width: 60%;
}

 .sent_msg p {
  background: #05728f none repeat scroll 0 0;
  border-radius: 3px;
  font-size: 14px;
  margin: 0; color:#fff;
  padding: 5px 10px 5px 12px;
  width:100%;
}
.outgoing_msg{ overflow:hidden; margin:26px 0 26px;}
.sent_msg {
  float: right;
  width: 46%;
}
.input_msg_write input {
  background: rgba(0, 0, 0, 0) none repeat scroll 0 0;
  border: medium none;
  color: #4c4c4c;
  font-size: 15px;
  min-height: 48px;
  width: 100%;
}

.type_msg {border-top: 1px solid #c4c4c4;position: relative;}
.msg_send_btn {
  background: #05728f none repeat scroll 0 0;
  border: medium none;
  border-radius: 50%;
  color: #fff;
  cursor: pointer;
  font-size: 17px;
  height: 33px;
  position: absolute;
  right: 0;
  top: 11px;
  width: 33px;
}
.messaging { padding: 0 0 50px 0;}
.msg_history {
  height: 516px;
  overflow-y: auto;
}
</style>
