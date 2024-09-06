<script>
  import { onMount } from 'svelte';

  let email = '';
  let password = '';
  let rememberMe = true;
  let showPassword = false;
  let errorMail = '';
  let errorPassword = '';

  function validarEMail() {
    const emailPattern = /^[a-z0-9]+@[a-z]+\.[a-z]{2,3}$/;
    if (!email) {
      errorMail = 'Debes introducir un mail';
      return false;
    } else if (!emailPattern.test(email)) {
      errorMail = 'El mail debe de ser válido';
      return false;
    }
    errorMail = '';
    return true;
  }

  function validarContrasinal() {
    const regexNumero = /[0-9]/;
    const regexMayus = /[A-Z]/;
    const regexSimbolo = /\W/;

    if (!password) {
      errorPassword = 'Debes introducir una contraseña';
      return false;
    } else if (!password.match(regexMayus) || !password.match(regexNumero) || !password.match(regexSimbolo)) {
      if (!regexMayus.test(password)) {
        errorPassword = 'La contraseña debe tener al menos una mayúscula';
      } else if (!regexNumero.test(password)) {
        errorPassword = 'La contraseña debe tener al menos un número';
      } else if (!regexSimbolo.test(password)) {
        errorPassword = 'La contraseña debe tener al menos un símbolo';
      }
      return false;
    } else if (password.length < 6 || password.length > 12) {
      errorPassword = 'La contraseña debe tener entre 6 y 12 caracteres';
      return false;
    }
    errorPassword = '';
    return true;
  }

  function validar(event) {
    event.preventDefault();
    if (validarEMail() && validarContrasinal()) {
      if (confirm('¿Desea acceder?')) {
        if (rememberMe) {
          localStorage.setItem('mail', email);
        }
        alert('Bienvenido a linktic');
      }
    }
  }

  function togglePasswordVisibility() {
    showPassword = !showPassword;
  }

  onMount(() => {
    const storedEmail = localStorage.getItem('mail');
    if (storedEmail) {
      email = storedEmail;
    }
  });
</script>

<main class="main-container">
  <section class="right">
    <h1 class="title">
      <i class="fa-brands fa-itunes-note"></i> Sign Into Account
      <i class="fa-brands fa-itunes-note"></i>
    </h1>
    <i class="fa-solid fa-compact-disc"></i>
    <form class="form-container" on:submit={validar}>
      <label class="username" for="mail">
        <i class="fa-solid fa-envelope"></i>
        <input
          id="mail"
          type="email"
          class="user-input"
          placeholder="E-Mail"
          bind:value={email}
          on:input={() => errorMail = ''}
          required
        />
        {#if errorMail}
          <p id="errorMail" class="errorMensajeForm">{errorMail}</p>
        {/if}
      </label>
      <div id="contra">
        <label class="password" for="contrasinal">
          <i class="fa-solid fa-lock"></i>
          <!-- Usamos dos inputs condicionales en lugar de un atributo dinámico -->
          {#if showPassword}
            <input
              id="contrasinal"
              type="text"
              class="pass-input"
              placeholder="Password"
              bind:value={password}
              on:input={() => errorPassword = ''}
              minlength="6"
              maxlength="12"
              required
            />
          {:else}
            <input
              id="contrasinal"
              type="password"
              class="pass-input"
              placeholder="Password"
              bind:value={password}
              on:input={() => errorPassword = ''}
              minlength="6"
              maxlength="12"
              required
            />
          {/if}
          <i
            class={showPassword ? 'fa-regular fa-eye' : 'fa-regular fa-eye-slash'}
            on:click={togglePasswordVisibility}
          ></i>
        </label>
        {#if errorPassword}
          <p id="errorContrasinal" class="errorMensajeForm">{errorPassword}</p>
        {/if}
      </div>
      <label id="labelRemember" class="keepme" for="remember">
        <input id="remember" type="checkbox" bind:checked={rememberMe} />
        <strong>Remember me</strong>
      </label>
      <button type="submit" id="enviar">Login</button>
    </form>
  </section>
</main>



