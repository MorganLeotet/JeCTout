<script>
// création d'une fonction pour faire apparaître la pop-up au clic
    function togglePopup() {
    let popup = document.querySelector('#popup');
    popup.classList.toggle('open');
}
</script>
<header>
    
    <nav>
        <div class="header_menu" id="menu">
            <button class="menu_btn" on:click={togglePopup}>Se connecter</button>
            <button class="menu_btn" on:click={togglePopup}>Inscription Gratuite</button>
        </div>

    </nav>
</header>

            <div id="popup">
                <div class="popup_content">
                    <h2>Se Connecter ou s'inscrire</h2>
                    <p class="popup_txt">Vous recevrez des réponses plus ciblées et plus faciles à comprendre pour vous, vu que vous serez connecté ou inscrit !!</p>
                    <a href="https://accounts.google.com/" class="popup_btn">Continuer avec Google</a>
                    <a href="https://login.live.com/" class="popup_btn">Continuer avec Microsoft</a>
                    <a href="https://appleid.apple.com/" class="popup_btn">Continuer avec Apple</a>
                    <input class="mail" type="mail" placeholder="Adresse e-mail">
                    <button class="mail_btn">Continuer</button>
                    <button class="popup_exit" on:click={togglePopup}>Fermer</button>
                </div>
            </div>