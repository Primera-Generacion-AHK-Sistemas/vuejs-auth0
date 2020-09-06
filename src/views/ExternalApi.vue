<template>
  <div>
    <div class="mb-5">
      <h1>API Spring Boot Local</h1>
      <p>
        Envía una solicitud POST a la API para registrar al usuario en la base de datos Postgres.
      </p>

      <button class="btn btn-primary mt-5" @click="callApi">Registrarse</button>
    </div>

    <div class="result-block-container">
      <div :class="['result-block', executed ? 'show' : '']">
        <h6 class="muted">Resultado</h6>
        <pre v-highlightjs="JSON.stringify(apiMessage, null, 2)">
          <code class="js rounded"></code>
        </pre>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Api",
  data() {
    return {
      apiMessage: null,
      executed: false
    };
  },
  methods: {
    async callApi() {
      const accessToken = await this.$auth.getTokenSilently();
      // eslint-disable-next-line
      console.log(`Bearer ${accessToken}`);

      try {
        // https://flask-auth0.azurewebsites.net/cotizacion-ccl?cotizacion=compra
        // http://127.0.0.1:5000/decode-jwt
        // http://127.0.0.1:5000/cotizacion-ccl?cotizacion=compra
        // http://127.0.0.1:3010/api/private
        // http://127.0.0.1:3010/api/users/signup

        const { data } = await this.$http.post("http://127.0.0.1:3010/api/user/signup", {},{
          headers: {
            Authorization: `Bearer ${accessToken}`
          }
        });

        this.apiMessage = data;
        this.executed = true;
      } catch (e) {
        this.apiMessage = `Error: the server responded with '${e.response.status}: ${e.response.statusText}'`;
      }
    }
  }
};
</script>

