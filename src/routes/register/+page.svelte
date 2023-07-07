<script lang="ts">

	//FORM VALIDATION

	import { useForm, Hint, HintGroup, validators, required, minLength, email } from "svelte-use-form";
	const form = useForm();
	
	function passwordMatch(value: string, form: any) {
		if (value !== form.values.password) {
			return { passwordMatch: true };
		}
	}

	const requiredMessage = "This field is required";
	
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

    async function mkRegistration(){
		if ($form.email && $form.password){
			const { data, error } = await supabase.auth.signUp({
			email: $form.email.value,
			password: $form.password.value,
			})
		}
    } 
    

</script>

{#if logStatus}
	<p>You should logout before register another account</p>
	<a href="/">Home</a>
{:else}
	<form use:form>
		<h1>
			Registration
		</h1>

		<label for="email">Email</label>
		<input type="email" name="email" use:validators={[required, email]} />
		<HintGroup for="email">
			<Hint on="required">{requiredMessage}</Hint>
			<Hint on="email" hideWhenRequired>This must be a valid email</Hint>	
		</HintGroup>

		<br><br>

		<label for="name">Name</label>
		<input type="text" name="name"  />

		<br><br>

		<label for="password">Password</label>
		<input type="password" name="password" use:validators={[required, minLength(5)]} />
		<HintGroup for="password">
			<Hint on="required">{requiredMessage}</Hint>
			<Hint on="minLength" hideWhenRequired let:value>This field must have at least {value} characters.</Hint>	
			<Hint on="containNumbers" hideWhen="minLength" let:value>
				This field must contain at least {value} numbers.
			</Hint>	
		</HintGroup>

		<br><br>

		<label for="passwordConfirmation">Password Confirmation</label>
		<input type="password" name="passwordConfirmation" use:validators={[required, passwordMatch]} />
		<HintGroup for="passwordConfirmation">
			<Hint on="required">{requiredMessage}</Hint>
			<Hint on="passwordMatch" hideWhenRequired>Passwords do not match</Hint>	
		</HintGroup><br />

		<br><br>

		<button disabled={!$form.valid} on:click|preventDefault={mkRegistration}>
			Register
		</button>
	</form>
{/if}

<style>
	:global(.touched:invalid) {
		border-color: red;
		outline-color: red;
	}
</style>