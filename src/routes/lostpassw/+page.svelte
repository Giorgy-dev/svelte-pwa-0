<script lang="ts">

  const linkForResetPassw = 'http://localhost:5173/updatepassw';

  //FORM VALIDATION

  import { useForm, validators, HintGroup, Hint, email, required } from "svelte-use-form";
  
  const form = useForm();

  //DB CONNECTION

  import { createClient } from '@supabase/supabase-js'

  const supabase = createClient(
      'https://jsqojknvwbllcslqdryx.supabase.co', 
      'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImpzcW9qa252d2JsbGNzbHFkcnl4Iiwicm9sZSI6ImFub24iLCJpYXQiOjE2ODg2NDc5NjQsImV4cCI6MjAwNDIyMzk2NH0.DmDZH5FiRZ8FrDYZKHztYJ8s9NSxRD3hJGIfdMnKy3M'
  )

  
  
  //! LOGIN THROUGH SESSION CHECK
  //@param: logStatus = boolean
  async function getSession(){
      return await supabase.auth.getSession()
    }
    $: logStatus = getSession() != null;
    supabase.auth.onAuthStateChange((e, session) => {
        logStatus = session != null
    })


    //FORGET PASSWORD
    async function mkReset(){
        if ($form.email) await supabase.auth.resetPasswordForEmail($form.email.value, {
          redirectTo: linkForResetPassw,
        });
    }

</script>



{#if logStatus}
<p>Oops! It looks like you already logged in</p>
<br><br>
<a href="/">Home</a>
{:else}
<form use:form>
    <h1>PASSWORD LOST</h1>

  <input type="email" name="email" use:validators={[required, email]} />
  <HintGroup for="email">
    <Hint on="required">This is a mandatory field</Hint>
    <Hint on="email" hideWhenRequired>Email is not valid</Hint>
  </HintGroup>

  <button disabled={!$form.valid} on:click={mkReset}>Request to reset password</button>
</form>
{/if}
