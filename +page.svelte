export const actions: Actions = {
    order: async (event) => {
        const form = await event.request.formData();

        fetch("http://localhost:8000/run", {
            method: "POST",
            body: JSON.stringify({items: form.getAll("items")}),
            headers: {
                'Content-Type': 'application/json'
            }
        });
        
        redirect(302, "/order");
    }
}
