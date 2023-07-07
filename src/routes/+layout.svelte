<script lang="ts">
import { createClient } from '@supabase/supabase-js'

    const supabase = createClient(
        'https://jsqojknvwbllcslqdryx.supabase.co', 
        'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImpzcW9qa252d2JsbGNzbHFkcnl4Iiwicm9sZSI6ImFub24iLCJpYXQiOjE2ODg2NDc5NjQsImV4cCI6MjAwNDIyMzk2NH0.DmDZH5FiRZ8FrDYZKHztYJ8s9NSxRD3hJGIfdMnKy3M'
    )

    
    async function logout(){
        if (getSession() != null){
            await supabase.auth.signOut()
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

</script>

<nav>
    <a href="/">Home</a>
    {#if logStatus}
        <a href="/" on:click={logout}>Logout</a>
    {:else}
        <a href="/login">Login</a>
    {/if}
    <a href="/register">Register</a>
    <p>status: {logStatus}</p>
</nav>

<slot></slot>

<style>
    :global(body) {
        margin: 0;
    }
    nav{
        display: flex;
        align-items: center;
        gap: 2rem;
        height: 80px;
        padding-left: 2rem;
        padding-right: 2rem;
        background-color: #dad7cd;
        color: #3a5a40;
    }
    nav>a{
        text-decoration: none;
        color: inherit;
    }
</style>
