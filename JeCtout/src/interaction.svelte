<script>
    const keyMistral = "hbqDh1Uflhfkc9RVoVAZLDuafI0If3k1";

    fetch("https://api.mistral.ai/v1/chat/completions", {
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
        content: " Svelte c'est la loose ? "
        }]
    })
})
        .then(response => response.json())
        .then(data => {
        console.log(data.choices[0].messages);
        });
    </script>

