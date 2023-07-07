<script>
    import { useForm, validators, HintGroup, Hint, email, required } from "svelte-use-form";
  
    const form = useForm();

    //DB CONNECTION

    import { createClient } from '@supabase/supabase-js'

    const supabase = createClient(
        'https://jsqojknvwbllcslqdryx.supabase.co', 
        'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImpzcW9qa252d2JsbGNzbHFkcnl4Iiwicm9sZSI6ImFub24iLCJpYXQiOjE2ODg2NDc5NjQsImV4cCI6MjAwNDIyMzk2NH0.DmDZH5FiRZ8FrDYZKHztYJ8s9NSxRD3hJGIfdMnKy3M'
    )

    async function mkLogin(){
		if ($form.email && $form.password){
			const { data, error } = await supabase.auth.signInWithPassword({
			email: $form.email.value,
			password: $form.password.value,
			})
		}
    } 

    //! LOGIN THROUGH SESSION CHECK
    //@param: logStatus = boolean
    async function getSession(){
        return await supabase.auth.getSession()
    }
    $: logStatus = getSession() != null;
    supabase.auth.onAuthStateChange((e, session) => {
        logStatus = session != null
    })

    async function logout(){
        if (getSession() != null){
            await supabase.auth.signOut()
        }
    }
  </script>
  
  {#if logStatus}
    <p>You're already logged in</p>
    <br><br>
    <a href="/">Home</a>
    <br><br>
    <a href="/" on:click={logout}>Logout</a>
  {:else}
    <form use:form>
      <h1>Login</h1>
    
      <input type="email" name="email" use:validators={[required, email]} />
      <HintGroup for="email">
        <Hint on="required">This is a mandatory field</Hint>
        <Hint on="email" hideWhenRequired>Email is not valid</Hint>
      </HintGroup>
    
      <input type="password" name="password" use:validators={[required]} />
      <Hint for="password" on="required">This is a mandatory field</Hint>
    
      <button disabled={!$form.valid} on:click={mkLogin}>Login</button>
    </form>

    <br><br>

    <a href="/lostpassw">forget password?</a>
  {/if}

  <style>
      :global(.touched:invalid) {
          border-color: red;
          outline-color: red;
      }
  </style>