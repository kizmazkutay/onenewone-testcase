<script setup>
import {ref} from 'vue'
import "@/components/ContentAction.vue";
</script>
<template>
  <div class="cardWrap">
    <div class="card mb-4 ">
      <div class="card-body">
        <div class="mb-3">
          <textarea class="form-control" v-model="newContent" id="contentText" placeholder="Your text here"
                    rows="3"></textarea>
        </div>
        <div class="buttonWrap">
          <div class="buttonIcons">
            <div class="buttonIcons1 me-2">
              <i class="fa-regular fa-star"></i>
            </div>
            <div class="buttonIcons1">
              <i class="fa-regular fa-face-smile"></i>
            </div>
          </div>
          <div class="buttonSubmit">
            <button type="button" class="btn btn-primary" @click="addContent()">Submit</button>
          </div>
        </div>
      </div>
    </div>
    <div id="todoList" v-for="item in itemsArray" :key="item.id">
      <div class="listItem">
        <div class="card mb-2">
          <div class="card-body">
            <div class="writeWrap">
              <img class="profile-img" src="/images/kolay.png">
              <div class="write-area">
                <div class="timer">{{ item.date }}</div>
                <div class="profile">Kutay</div>
                {{ item.content }}
              </div>
            </div>
            <hr>
            <div class="buttonIcons">
              <div class="button">
                <button class="icon-none p-1" @click="likeContent(item.id)"><i class="fa-solid fa-thumbs-up"></i>
                </button>
                {{ item.like }}
                <button class="icon-none p-1" @click="dislikeContent(item.id)"><i class="fa-solid fa-thumbs-down"></i>
                </button>
                {{ item.dislike }}
              </div>
              <div class="buttonIcons1">
                <button class="icon-none" @click="removeContent(item.id)"><i class=" fa-regular fa-trash-can"></i>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>
<script>
import {onMounted, ref} from "vue";

export default {}
onMounted(() =>
    addContent(),
)
let itemsArray = ref([]);
const newContent = ref('');
const contentId = ref(0);
const like = ref(0);
const dislike = ref(0);
const addContent = () => {
  itemsArray.value.push({
    id: contentId.value++,
    content: newContent.value || ' ',
    like: 0,
    dislike: 0,
    date: new Date().toLocaleString()
  });
  writeLocalStorage();
}

const removeContent = (id) => {
  itemsArray.value.splice(itemsArray.value.findIndex(c => c.id === id), 1);
  writeLocalStorage();
};

const likeContent = (id) => {
  itemsArray.value = itemsArray.value.map(content => {
    if (content.id === id) {
      content.like++;
    }
    return content;
  });
  writeLocalStorage()
};

const dislikeContent = (id) => {
  itemsArray.value = itemsArray.value.map(content => {
    if (content.id === id) {
      content.dislike++;
    }
    return content;
  });
  writeLocalStorage()
};

const writeLocalStorage = () => {
  localStorage.setItem('contentList', JSON.stringify(itemsArray.value));
};

const readLocalStorage = () => {
  const data = localStorage.getItem('contentList');
  if (data) {
    itemsArray.value = JSON.parse(data);
    contentId.value = itemsArray.value.length;
  }
};

readLocalStorage();
</script>
<style>
.cardWrap {
  display: flex;
  flex-direction: column;
  width: 500px;
  height: 500px;
  margin-top: 50px;
}

.buttonIcons1 {
  display: flex;
  flex-direction: row;
  justify-content: end;
}

.buttonWrap {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

.buttonIcons {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.icon-none {
  padding: 0;
  border: none;
  background: none;
}

.profile-img {
  width: 70px;
  height: 70px;
}

.writeWrap {
  display: flex;
  flex-direction: row;
  justify-content: start;
}

.timer {
  font-size: 12px;
  margin-top: 5px;
}

.profile {
  color: rebeccapurple;
  font-weight: bold;
}
</style>


