<form id="labInfo">
<label for="labID">Lab POD</label><br>
<input type="text" id="pod" name="POD"> <br>
<button onclick="update()">Update</button>
</form>

<script>
async function update(){
    event.preventDefault()
    response = await fetch(`https://63f62bf859c944921f6e89de.mockapi.io/ivrpod?POD=SOL${document.forms.labInfo[0].value}`,
    {
    method: 'GET',
    redirect: 'follow'
})
response = await response.json()
await sessionStorage.setItem("EPDN",await response[0]['EP DN'])
await sessionStorage.setItem("login",await response[0]['Admin username'])
await sessionStorage.setItem("EPname",await response[0].EP)
await sessionStorage.setItem("Queue",await response[0].Queue)
await sessionStorage.setItem("Team",await response[0].Team)
await sessionStorage.setItem("WxC",await response[0].WxC)
await sessionStorage.setItem("PW",await response[0].Password)
await sessionStorage.setItem("POD",await response[0].POD)
}

</script>

[Start the lab](../)


