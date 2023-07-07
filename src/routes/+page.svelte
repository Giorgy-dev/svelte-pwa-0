<script lang="ts">
    import { createClient } from '@supabase/supabase-js'

    const supabase = createClient(
        'https://jsqojknvwbllcslqdryx.supabase.co', 
        'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImpzcW9qa252d2JsbGNzbHFkcnl4Iiwicm9sZSI6ImFub24iLCJpYXQiOjE2ODg2NDc5NjQsImV4cCI6MjAwNDIyMzk2NH0.DmDZH5FiRZ8FrDYZKHztYJ8s9NSxRD3hJGIfdMnKy3M'
    )
    
    async function getSession(){
        return await supabase.auth.getSession()
    }
</script>

<h1>Home</h1>

