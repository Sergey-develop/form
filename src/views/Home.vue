<template>
  <form
    method="POST"
    @submit.prevent="submitForm"
    class="d-flex align-items-center justify-content-center"
  >
    <div class="parent">
      <h5>Форма подачи заявки в отдел сервиса и качества</h5>

      <form class="block">
        <div class="select-country">
          <label class="required">Ваш филиал</label>
          <b-select placeholder="Select a name">
            <option v-for="item in Cities" :key="item.id">
              {{ item.title }}
            </option>
          </b-select>
          <b-check class="check-box">Онлайн</b-check>
        </div>

        <div class="feedback flex-row-reverse align-items-start">
          <label class="required">Тема обращения</label>
          <div class="check-box-row">
            <div class="custom-control custom-checkbox">
              <input
                v-model="checkedFields"
                value="Недоволен качеством услуг"
                type="checkbox"
                class="custom-control-input"
                id="customCheck1"
              />
              <label class="custom-control-label" for="customCheck1">
                Недоволен качеством услуг
              </label>
            </div>

            <div class="custom-control custom-checkbox">
              <input
                v-model="checkedFields"
                value="Расторжение договора"
                type="checkbox"
                class="custom-control-input"
                id="customCheck2"
              />
              <label class="custom-control-label" for="customCheck2">
                Расторжение договора
              </label>
            </div>

            <div class="custom-control custom-checkbox">
              <input
                v-model="checkedFields"
                value="Не приходит письмо активации на почту"
                type="checkbox"
                class="custom-control-input"
                id="customCheck3"
              />
              <label class="custom-control-label" for="customCheck3">
                Не приходит письмо активации на почту
              </label>
            </div>

            <div class="custom-control custom-checkbox">
              <input
                v-model="checkedFields"
                value="Не работает личный кабинет"
                type="checkbox"
                class="custom-control-input"
                id="customCheck4"
              />
              <label class="custom-control-label" for="customCheck4">
                Не работает личный кабинет
              </label>
            </div>
          </div>
          <b-input placeholder="Другое" />
        </div>

        <div class="description">
          <label class="required">Описание проблемы</label>
          <b-textarea
            v-model="description"
            required
            placeholder="Введите текст"
          />
        </div>

        <div class="files">
          <label> Загрузка документов </label>

          <div class="deck">
            <div>Приложите пожалуйста, полноэкранный скриншот.</div>
            <div>Это поможет быстрее решить проблему.</div>

            <input
              type="file"
              @change="processFile($event)"
              name="photo"
              multiple
              accept="image/*,image/jpeg"
            />
          </div>
        </div>

        <div>
          <b-button type="submit">Отправить</b-button>
        </div>
      </form>
    </div>
  </form>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "@/components/HelloWorld.vue";
import axios from "axios";
interface CategoryItem {
  id: number;
  title: string;
}
@Component({
  components: {
    HelloWorld,
  },
})
export default class Home extends Vue {
  Cities: Array<CategoryItem> = [];
  description: undefined;
  image: undefined;

  data() {
    return {
      description: undefined,
      image: "",
      checkedFields: [],
    };
  }

  private processFile(event: any) {
    this.image = event.target.files[0];
  }

  private async foo() {
    let result = await fetch(
      "https://60254fac36244d001797bfe8.mockapi.io/api/v1/city"
    );
    this.Cities = result.ok ? await result.json() : [];
  }

  private mounted(): void {
    this.foo();
  }

  private submitForm() {
    let data = {
      param1: this.param1,
    };

    axios
      .post(
        "https://60254fac36244d001797bfe8.mockapi.io/api/v1/send-form.",
        data
      )
      .then((res) => {
        if (res.data.check == true) {
          this.$refs.form.submit();
        } else {
          console.log("some error");
        }
      });
  }
}
</script>

<style scoped lang="scss">
.required {
  &:after {
    content: "*";
    color: red;
  }
}

.check-box {
  padding-bottom: 5px;
  padding-top: 10px;
  font-size: 13px;
}

.parent {
  text-align: start;
  padding-top: 10px;
  margin-left: auto;
  margin-right: auto;
  width: 48%;

  h5 {
    text-align: start;
    font-weight: 900;
    padding-bottom: 14px;
  }
}

.block {
  border: 1px solid black;
  border-radius: 4px;
  padding: 33px 36px;
  align-items: start;
}

.select-country {
  padding-bottom: 30px;
  width: fit-content;
  label {
    font-size: 13px;
  }
}

.feedback {
  padding-bottom: 30px;
  width: fit-content;
  label {
    font-size: 13px;
    padding: 5px 0;
  }
}

.description {
  padding-bottom: 30px;
  label {
    font-size: 15px;
    padding: 5px 0;
  }
}

.files {
  padding-bottom: 30px;
  .deck {
    font-size: 12px;
    color: gray;
    margin: 10px 0;
  }
}

.check-box-row {
  .custom-control.custom-checkbox {
    margin: 7px 0;
    font-size: 13px;
  }
}

.custom-checkbox .custom-control-label::before {
  border-radius: 999px;
}
</style>
