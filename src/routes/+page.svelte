<script lang="ts">
  import Container from "$lib/Container.svelte";
  import { Button, Snackbar, TextField } from "noph-ui";

  
  function login() {
    if (hideInvalidCredentials) hideInvalidCredentials();
    console.log(username, password);
    if (username == 'anonymous' && password == 'rumpelstielschen') {
      passwordError = userError = false;
    } else {
      userError = passwordError = true;
      if (showInvalidCredentials) showInvalidCredentials();
    }
    
    // goto('/login');
  }

  let showInvalidCredentials = $state<() => void | undefined>();
  let hideInvalidCredentials = $state<() => void | undefined>();
  let username = $state('');
  let password = $state('');
  let userError = $state(false);
  let passwordError = $state(false);
</script>
<Snackbar
  bind:showPopover={showInvalidCredentials}
  bind:hidePopover={hideInvalidCredentials}
  id="invalid_cred"
  label="Invalid credentials"
  --np-snackbar-container-color="var(--np-color-error)"
  >
</Snackbar>

<div class="center-container">
  <Container>
    <h1 class="login-title">YourProject Login</h1>
    <h4>Please provide your credentials </h4>
    <form onsubmit={login} style:width="100%">
      <div class="form-field">
        <TextField
          variant="outlined"
          label="Username"
          type="text"
          bind:value={username}
          issues={userError ? [{message: 'invalid credentials'}] : []}
        />
      </div>
      <div class="form-field" style:width="100%">
        <TextField
          variant="outlined"
          label="Password"
          type="password"
          bind:value={password}
          issues={passwordError ? [{message: 'invalid credentials'}] : []}
        />
      </div>
      <Button onclick={login} style="width: 100%;">Login</Button>
    </form>
  </Container>
</div>

<style>
  .center-container {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
  }

  .login-title {
    color: var(--md-sys-color-on-surface);
    font-family: 'Roboto', sans-serif;
  }

  .form-field {
    width: 100%;
    margin-bottom: 1.5rem;
  }

  :global(.form-field label) {
    width: 100%;
  }

</style>