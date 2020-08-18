<template>
<v-app>
  <v-card class="overflow-hidden">
    <v-app-bar
      absolute
      color="#fcb69f"
      dark
      shrink-on-scroll
      src="https://picsum.photos/1920/1080?random"
      scroll-target="#scrolling-techniques-2"
    >
      <template v-slot:img="{ props }">
        <v-img
          v-bind="props"
          gradient="to top right, rgba(19,84,122,.5), rgba(128,208,199,.8)"
        ></v-img>
      </template>

      <v-app-bar-nav-icon @click="openDrawer"></v-app-bar-nav-icon>

      <v-toolbar-title><b>Yappin Chats</b> (Version Aiden)</v-toolbar-title>

    </v-app-bar>
    <v-sheet
      id="scrolling-techniques-2"
      class="overflow-y-auto"
      max-height= "800"
      color=""
    >
      <v-spacer id="topSpacer"></v-spacer>
        <v-card
            tile
            v-for="(chat, index) in chats"
            :key="index">
          <v-card-text>
            <b>{{chat.user}}</b> : {{chat.message}}
          </v-card-text>
        </v-card>

    </v-sheet>
  </v-card>
    <v-footer
        app
        tile
        padless
        height="100"
        dark>
        <v-card
            dark
            width="100%"
            tile
            flat>
            <v-card-text>
                <v-text-field
                    v-model="text"
                    append-outer-icon="send"
                    @click:append-outer="sendMessage()"
                    @keypress.enter="sendMessage()" />
            </v-card-text>
        </v-card>
    </v-footer>

  <v-navigation-drawer
        v-model="drawer"
        absolute
        temporary
        dark>
        <v-container>

            <v-list
                dense
                nav
                class="py-0">
                <v-list-item>
                    <v-file-input
                      label="File input"
                      prepend-icon="mdi-camera"
                      hide-input
                     ></v-file-input>
                    <v-list-item-title> {{userName}}
                    </v-list-item-title>
                </v-list-item>

                <v-list-item>
                    <v-list-item-icon>
                        <v-icon>mdi-account-convert-outline</v-icon>
                    </v-list-item-icon>
                    <v-list-item-title>Change Username
                    </v-list-item-title>
                </v-list-item>
                <v-card
                    outlined
                    dark>
                    <v-card-text>
                        <v-text-field
                            placeholder="Enter New Username"
                            hint="Make sure it's catchy"
                            dense
                            clearable
                            v-model="userNameEntry"
                            append-outer-icon="send"
                            @click:append-outer="newUserName()"
                            @keypress.enter="newUserName()"
                            padless>
                        </v-text-field>
                    </v-card-text>
                </v-card>
            </v-list>
        </v-container>
    </v-navigation-drawer>
</v-app>
</template>

<script>
import axios from 'axios'
String.prototype.replaceAt = function (index, replacement) {
    return this.substr(0, index) + replacement + this.substr(index + replacement.length)
}


export default {
    name: 'App',
    data() {
        return {
            windowHeight: window.screen.height,
            userName: 'Anonymous',
            userImage: new Image(50,50),
            userNameEntry: '',
            chats: [{
                    user: 'Aric',
                    message: 'Ok this is something, you happy?'
                },
                {
                    user: 'Daniel',
                    message: 'What is this place? I\'m kinda freaked out man. Let`s go, jogger'
                },
            ],
            text: '',
            // badWords: ['two genders', 'jogger'],
            badWords2: [{
                    word: 'pro-trudeau',
                    stars: 5
                },
                {
                    word: 'liberal',
                    stars: 2
                },
            ],
            drawer: false,
        }
    },
    methods: {
        sendMessage() {
            let chat = {
                user: this.userName,
                message: this.text
            }
            this.chats.push(chat)
            this.text = ''
        },
        newUserName() {
            this.userName = this.userNameEntry
        },
        openDrawer(){
          this.drawer = !this.drawer
        }
        //   addUser() {
        //       this.username.push(this)
        //   }
    },
    computed: {
        censorchats() {
            let k
            let i //this indexes the word we are on
            for (k = 0; k < this.chats.length; k++) {
                for (let l = 0; l < this.badWords2.length; l++) {
                    for (i = 0; i < this.chats[k].length; i++) {
                        let badWord = ''
                        let jogger = this.badWords[l].word
                        if (this.chats[k][i] == this.badWords[l].word[0]) {
                            badWord = this.badWords[l].word[0]
                            for (let j = 0; j < this.badWords[l].word.length - 1; j++) {
                                let letter = this.chats[k][i + j + 1]
                                if (letter == jogger[j + 1]) {
                                    badWord += this.chats[k][i + j + 1]
                                    console.log(badWord)
                                } else {
                                    break
                                }
                            }
                        }
                        if (badWord == this.badWords[l].word) {
                            for (let s = 0; s < this.badWords[l].stars; s++) {
                                chats[k].message = chats[k].message.replaceAt(i + s, '*')
                            }
                        }
                    }
                }
            }
            return chats
        }
    },
    //   mounted ()
    //   {
    // let any = '<div class="v-card v-sheet theme--light rounded-0"><div class="v-card__text"> I am the first chat </div></div>'
    // let el = document.getElementById('chatbox')
    // el.innerHTML += any
    // axios.get('http://localhost:3000')
    //   .then(function (response) {
    //     console.log(response.data);
    //   })
    //   }
};
</script>

<style>
html {
    overflow: hidden !important;
}
#topSpacer{
  padding-top: 130px;
}
</style>
