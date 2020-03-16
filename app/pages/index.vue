<template>
  <section class="home">
    <div class="py-24 md:py-36 mx-auto flex flex-wrap flex-col md:flex-row items-center">
      <div class="flex flex-col w-full xl:w-3/5 justify-center lg:items-start overflow-y-hidden">
        <div v-html="$md.render(welcomeText)" class="home__welcome markdown" />

        <div class="mb-12 xl:mb-0">          
          
          
          <h4 v-if="isSignedUp">Thank you - we'll be in touch shortly.</h4>
          <form
            v-else
            @submit.prevent="handleSubmit"
            name="signups"
            netlify
            class="flex items-center border-b border-b-2 border-blue-400 py-2"
          >
            <input
              ref="emailInput"
              v-model="form.email"
              class="appearance-none mb-36 bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none"
              type="text"
              name="email"
              placeholder="your@email.com"
              aria-label="Email address"
            />

            <button
              class="flex-shrink-0 bg-blue-500 hover:bg-blue-700 border-blue-500 hover:border-blue-700 text-sm border-4 text-white py-1 px-2 rounded"
              type="submit"
            >
              Subscribe to mailing list
            </button>
          </form>

          
          
         
        </div>
      </div>
      <div class="flex flex-col w-full xl:w-2/5">
        <img class="rounded shadow-xl" style="
    margin-left: 2rem;
    margin-right: 2rem;" src="https://datemymate.fun/images/uploads/home.svg" />
      </div>      
      
    </div>
  </section>
  
  
  <section>
    <div class="flex flex-wrap md:-mx-4 pb-20"><div class="w-full md:w-1/2 my-4 md:px-4"><div class="post"><a href="/blog/meditation-pop-up-forage" class=""><img src="https://datemymate.fun/images/uploads/dmm-min.jpeg" class="w-full"> <div class="p-6 bg-white"><h2 class="text-2xl mb-2">Meditation pop-up forage</h2> <p class="text-base font-light">
              Lorem ipsum dolor amet lo-fi vice flannel, distillery bicycle rights bitters sartorial raw denim pop-up succulents offal williamsburg iPhone gastropub...
            </p> <h6 class="text-blue-600 mt-4 font-medium">Read more</h6></div></a></div></div><div class="w-full md:w-1/2 my-4 md:px-4"><div class="post"><a href="/blog/pug-swag" class=""><img src="https://source.unsplash.com/random/640x340" class="w-full"> <div class="p-6 bg-white"><h2 class="text-2xl mb-2">Pug Swag</h2> <p class="text-base font-light">
              Pug swag yuccie artisan hot chicken, glossier cold-pressed. Tumeric you probably haven't heard of them kinfolk yuccie. Cronut prism jean shorts, pickl...
            </p> <h6 class="text-blue-600 mt-4 font-medium">Read more</h6></div></a></div></div></div>
</section>
  
  
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import settings from '@/content/settings/general.json';

@Component({
  // Called to know which transition to apply
  transition() {
    return 'slide-left';
  },
})
export default class Home extends Vue {
  welcomeText = settings.welcomeText;

  get posts(): Post[] {
    return this.$store.state.posts;
  }

  isSignedUp = false;

  form = {
    email: '',
  };

  encode(data): string {
    return Object.keys(data)
      .map(key => `${encodeURIComponent(key)}=${encodeURIComponent(data[key])}`)
      .join('&');
  }

  validEmail(email): boolean {
    // eslint-disable-next-line
    const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return re.test(email);
  }

  async handleSubmit(): Promise<void> {
    if (!this.validEmail(this.form.email)) {
      this.$refs.emailInput.focus();
      return;
    }

    try {
      await fetch('/', {
        method: 'POST',
        headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
        body: this.encode({ 'form-name': 'signups', ...this.form }),
      });

      this.isSignedUp = true;
    } catch (error) {
      console.error(error);
    }
  }
}
</script>
