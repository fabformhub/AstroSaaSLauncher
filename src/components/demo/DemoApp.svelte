<script>
  import { createClient } from '@supabase/supabase-js';

  const supabase = createClient(
    import.meta.env.PUBLIC_SUPABASE_URL,
    import.meta.env.PUBLIC_SUPABASE_ANON_KEY
  );

  let email = "";
  let password = "";
  let user = null;
  let error = "";

  supabase.auth.getUser().then(({ data }) => {
    user = data.user;
  });

  async function login() {
    error = "";
    const { data, error: err } = await supabase.auth.signInWithPassword({
      email,
      password
    });
    if (err) error = err.message;
    user = data.user;
  }

  async function signup() {
    error = "";
    const { data, error: err } = await supabase.auth.signUp({
      email,
      password
    });
    if (err) error = err.message;
    user = data.user;
  }

  async function logout() {
    await supabase.auth.signOut();
    user = null;
  }
</script>

{#if !user}
  <div class="border-4 border-black bg-white shadow-[12px_12px_0_0_#000] p-12 space-y-8">

    <h2 class="text-4xl font-black">Try the Live Demo</h2>

    {#if error}
      <p class="text-red-600 font-bold">{error}</p>
    {/if}

    <input
      type="email"
      placeholder="Email"
      bind:value={email}
      class="w-full border-4 border-black px-4 py-3 text-xl"
    />

    <input
      type="password"
      placeholder="Password"
      bind:value={password}
      class="w-full border-4 border-black px-4 py-3 text-xl"
    />

    <div class="flex gap-6">
      <button on:click={login}
        class="px-10 py-4 text-xl font-black bg-black text-white border-4 border-black shadow-[6px_6px_0_0_#000]">
        Login
      </button>

      <button on:click={signup}
        class="px-10 py-4 text-xl font-black bg-[#FF5F7E] text-white border-4 border-black shadow-[6px_6px_0_0_#000]">
        Sign Up
      </button>
    </div>

  </div>

{:else}

  <div class="border-4 border-black bg-white shadow-[12px_12px_0_0_#000] p-12 space-y-8">

    <h2 class="text-4xl font-black">Dashboard Preview</h2>

    <p class="text-xl text-[#475569]">
      You are logged in as <strong>{user.email}</strong>.
    </p>

    <p class="text-xl text-[#475569]">
      This is a live authenticated view powered by Supabase Auth.
    </p>

    <button on:click={logout}
      class="px-10 py-4 text-xl font-black bg-black text-white border-4 border-black shadow-[6px_6px_0_0_#000]">
      Logout
    </button>

  </div>

{/if}

