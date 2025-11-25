<script>
    import Popup from "./Popup.svelte";
    import SideBloc from "./sideBloc.svelte";
</script>

<header>

    <nav>
        <Popup />
        
    </nav>
</header>

<main class="container">
    <SideBloc />
    <div class="bloc_img">
        <img class="logo_img" src="/logo/JeCtout.jpg" alt="logo de l'application">
    </div>
</main>
<footer>
    <div class="footer_message">
    <p>En envoyant des messages à JeCTout, vous confirmez que votre ignorance et acceptez par conséquent la parole unique de cette application</p>
    </div>
</footer>

<style>
</style>
