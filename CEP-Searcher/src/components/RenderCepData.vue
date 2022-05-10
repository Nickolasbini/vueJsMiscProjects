<script>
  export default {
    props: {
      cepData: Object
    },
  }
</script>

<template>
  <div class="table-responsive-sm mt-5 p-2 border shadow">
    <p class="h4 p-2">
      Results for: {{ cepData.cep }}
    </p>
    <table class="table table-hover">
      <thead>
        <tr>
          <th>Street</th>
          <th>Complement</th>
          <th>Neighborhood</th>
          <th>City</th>
          <th>Uf</th>
        </tr>
      </thead>
      <tbody>
          <tr>
            <td>{{ cepData.logradouro  || "..." }}</td>
            <td>{{ cepData.complemento || "..." }}</td>
            <td>{{ cepData.bairro      || "..." }}</td>
            <td>{{ cepData.localidade  || "..." }}</td>
            <td>{{ cepData.uf          || "..." }}</td>
          </tr>
      </tbody>
    </table>
  </div>
</template>