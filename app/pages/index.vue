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
      <div class="flex flex-col w-full xl:w-2/5" style="
    text-align: -webkit-center;
">
        <img class="rounded shadow-xl" style="
        max-width: 80%;
  " src="https://datemymate.fun/images/uploads/home.svg" />
      </div>      
      
    </div>
    
    
    
    
    
    <div class="flex flex-wrap md:-mx-4 pb-20">
   <div class="w-full md:w-1/2 my-4 md:px-4 shadow-xl rounded">
      <div class="post">
         <a href="https://airtable.com/shrc3DiW9MCRdFGFV" class="">
            <img src="https://datemymate.fun/images/uploads/dmm-min.jpeg" class="w-full rounded"> 
            <div class="p-6 bg-white">
               <h2 class="text-2xl mb-2">Get in our Insta</h2>
               <p class="text-base font-light">
Get yourself, or your single mate out there in front of the Date My Mate Instagram audience.  Analytically speaking, you've got a good chance someone will slide into your or your mate's DMs.  Submit now, and watch the hype!</p>
               <h6 class="text-blue-600 mt-4 font-medium">Submit now</h6>
            </div>
         </a>
      </div>
   </div>
   <div class="w-full md:w-1/2 my-4 md:px-4 shadow-xl rounded">
      <div class="post">
         <a href="https://airtable.com/shrWXjpf0eKzssPi3" class="">
            <img src="https://datemymate.fun/images/uploads/side.jpg" class="w-full rounded"> 
            <div class="p-6 bg-white">
               <h2 class="text-2xl mb-2">Bring the heat</h2>
               <p class="text-base font-light">
 A great pitch is basically the pinnacle of wingmanning/wingwomanning your close mate, sibling or cousin. Not only does it give you the chance to lay out the cold hard facts on why your mate is perfect it also makes for an excellent night out.
               </p>
               <h6 class="text-blue-600 mt-4 font-medium">Apply to pitch</h6>
            </div>
         </a>
      </div>
   </div>
</div>
    
    
    
    
    
    
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
