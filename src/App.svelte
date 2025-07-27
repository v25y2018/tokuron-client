<script>
import { Router } from '@mateothegreat/svelte5-router';
import Register from "./routes/Register.svelte";
import Login from './routes/Login.svelte';
const routes = [
  {
    name: 'register',
    path: '/register',
    component: Register,
  },
  {
    name: 'login',
    path: '/login',
    component: Login,
  }
];
</script>

<Router {routes} />