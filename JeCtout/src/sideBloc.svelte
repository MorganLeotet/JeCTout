<script>
// scroll automatique (code récupérer d'une doc internet pour faire un scroll auto à chaque conversation)
    import { tick, onMount } from "svelte";
    let chatBox;



// fonction pour ouvrir et fermer la pop-up
function togglesideBloc() {
    let sideBloc =document.querySelector('#sideBloc');
    sideBloc.classList.toggle('open');
}

/* ======================== INTERACTION AVEC API =========================*/

// Définition des variables

// variable UserQuestion (question que pose l'utilsateur à l'IA)
let userQuestion = '';

// variable aiResponse (réponse de l'IA à la question utilisateur)
let aiResponse = '';

// variable chatContainer qui va stocker la question du user et la réponse de l'IA dans le t'chat
let chatContainer = [];

// variable chatHistory qui va stocker les conversations entre user et IA dans l'historique
let chatHistory = [];



// variable pour stocker la clé API
const keyMistral = "hbqDh1Uflhfkc9RVoVAZLDuafI0If3k1";

// foonction pour appeler l'API.              le try et catch ( aide de chat gpt car je n'arrivais pas à faire fonctionner le dialogue avec l'API)
async function getAiResponse(question) {
    try {
        const response = await fetch("https://api.mistral.ai/v1/chat/completions", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                "Authorization": `Bearer ${keyMistral}`
            },
            body: JSON.stringify({
            model: "mistral-tiny",
            messages: [
            {
                role: "user", 
                content: question
            }]
        })
    })
        const data = await response.json();
        console.log("Réponse API :", data);

        const aiText =  // ( code récupérer d'une doc internet j'avais un soucis d'affichage de la réponse de l'API)
                data.choices?.[0]?.message?.content || 
                data.choices?.[0]?.content || 
                data.choices?.[0]?.text || 
                "Réponse vide";

        return aiText;
        
    } catch (error) {
        console.error(" Absent ne laissez pas de message !!", error);
        return "Votre demande est nulle !!!";
    }
}
// function pour envoyer la question
async function ajoutQuestion(event) {
    event.preventDefault();
    const question = userQuestion;
// Récupérer la question user et l'afficher dans le t'chat
    chatContainer = [...chatContainer, {role: 'user', text: question}];
// Remise à zéro de la zone de texte
    userQuestion = '';
// Récupérer la réponse de l'IA
    aiResponse = await getAiResponse(question);
    chatContainer = [...chatContainer, {role: 'ai', text: aiResponse}];
// mise à jour du DOM ( code récupérer d'une doc sur internet)
    await tick();
    chatBox.scrollTop = chatBox.scrollHeight;

// stocke la conversation dans l'historique
    chatHistory = [...chatHistory, [...chatContainer]];
    chatContainer = [];
}

// fonction pour supprimer une conversation ( code récupérer d'une doc internet )
function supprConversation(index) {
    chatHistory = chatHistory.filter((_, i) => i !== index);
}


</script>

<!------------------------- Bloc Menu Burger------------------------------->
<div class="mbg">
    <ul class="burger_menu" id="burg_menu">
    <button type="button" aria-label="Ouvrir" onclick={togglesideBloc}>
        <span class="burger" id="burger">
            <span></span>
            <span></span>
            <span></span>
        </span>
    </button>
    </ul>
</div>
<!------------------------ Bloc Historique Conversation ------------------->       
<div id="sideBloc">
    <div class="sideBloc_content">
        <h2>Historique des Conversations</h2>
        <button class="sideBloc_exit" onclick={togglesideBloc}>Fermer</button>
        {#each chatHistory as chatContainer, index}
            <details class="historyItem">
                <summary>{chatContainer[0].text || 'Conversation'}
                    <button type="button" class="deleteBtn" onclick={() => supprConversation(index)}>🗑️</button>
                </summary>
                <section class="stockHisto">
                    {#each chatContainer as message}
                        {#if message.role === 'user'}
                            <article class="userMessage">
                                <p>{message.text}</p>
                            </article>
                        {:else}
                            <article class="aiResponse">
                                <p>{message.text}</p>
                            </article>
                        {/if}
                    {/each}
                </section>
            </details>
        {/each}
    </div>
</div>
<!------------------------- Bloc T'chat ------------------------------------->
<section class="bloc_chat" bind:this={chatBox}> 
    {#each chatHistory as conversation (conversation)}
        <div class="conversation">
            {#each conversation as msg (msg.role)}
                {#if msg.role === 'user'}
                    <div class="user">{msg.text}</div>
                {:else if msg.role === 'ai'}
                    <div class="ai">{msg.text}</div>
                {/if}
            {/each}
        </div>
    {/each}
    <div>
        {#each chatContainer as msg (msg.role)}
            <div class="conversation">
                {#if msg.role === 'user'}
                    <div class="user">{msg.text}</div>
                {:else if msg.role === 'ai'}
                    <div class="ai">{msg.text}</div>
                {/if}
            </div>
        {/each}
    </div>
</section>
<!------------------------- Bloc Bouton ------------------------------------->
<form onsubmit={ajoutQuestion} class="bloc_btn">
        <input id="new_conversation" name="new_conversation" type="text" placeholder=" Que puis-je faire pour toi ? " bind:value={userQuestion}>
        <button class="create_btn" type="submit">Créer</button>
</form>
