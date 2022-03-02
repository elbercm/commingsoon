<template>
  <!-- Coming Soon Area -->
  <section id="coming_soon_area" class="ptb-100">
    <div class="container">
      <div class="row">
        <div class="col-lg-8 offset-lg-2">
          <div class="coming_soon_content">
            <nuxt-link to="/"
              ><img src="~/assets/img/logo.png" alt="coming-soon"
            /></nuxt-link>
            <div class="coming_soon_title">
              <h2>Estamos em construção</h2>
              <p>
                Em breve um lugar seguro para quem trabalha na área da segurança
                pública fazer negócios, comprar e vender online sem correr
                riscos.
              </p>
            </div>
            <div class="coming_soon_time">
              <div id="countdown_soon">
                <Timer date="October 15, 2022" />
              </div>
            </div>
            <div class="coming_soon_newsletter">
              <h3>Cadastre-se para receber novidades!</h3>
              <form @submit="handleSubmit">
                <div class="input-group">
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Email"
                    id="email"
                    name="email"
                    v-model="email"
                    :class="{
                      'is-invalid': submitted && $v.email.$error,
                    }"
                  />
                  <button
                    type="button"
                    @click.prevent="handleSubmit"
                    class="theme-btn-one btn-black-overlay btn_md"
                  >
                    Inscreva-se
                  </button>
                  <div
                    v-if="submitted && $v.email.$error"
                    class="invalid-feedback"
                  >
                    <span v-if="!$v.email.email">Email inválido.</span>
                  </div>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Timer from "../components/widgets/Timer";
import { email } from "vuelidate/lib/validators";
import Swal from "sweetalert2";

const Toast = Swal.mixin({
  toast: true,
  position: "top-end",
  showConfirmButton: false,
  timer: 3000,
  timerProgressBar: true,
  didOpen: (toast) => {
    toast.addEventListener("mouseenter", Swal.stopTimer);
    toast.addEventListener("mouseleave", Swal.resumeTimer);
  },
});
export default {
  layout: "headless-layout",
  name: "coming-soon",

  components: {
    Timer,
  },

  data() {
    return {
      title: "DesapegaMike | Em Construção",
      email: "",
      submitted: false,
    };
  },
  validations: {
    email: { email },
  },
  mounted() {
    // For scroll page top for every Route
    window.scrollTo(0, 0);
  },
  methods: {
    handleSubmit(e) {
      console.log("cadastrarEmail");
      this.submitted = true;

      if (!this.$v.$invalid) {
        console.log("ENTROU EMAIL");
        const formData = new FormData();
        formData.append("email", this.email);
        console.log("ENTROU EMAIL", this.email);
        this.$axios
          .post("/api/v1/cadastro-newsletter", formData)
          .then((response) => {
            Toast.fire({
              icon: "success",
              title: "E-mail Cadastrado!",
            });
          })
          .catch((error) => {
            console.log(error);
          });
      } else {
        this.$v.$touch();
      }
      this.email = "";
    },
  },

  // Page head() Title, description for SEO
  head() {
    return {
      title: this.title,
      meta: [
        {
          hid: "description",
          name: "DesapegaMike | Em construção",
          content: "DesapegaMike | Seu lugar seguro para comprar e vender",
        },
      ],
    };
  },
};
</script>
