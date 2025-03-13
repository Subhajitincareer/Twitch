# **Twitch Stream Status**
A simple web application that displays the online/offline status of popular Twitch streamers. Users can filter the list to view all streamers, only online streamers, or only offline streamers.

## **Features**
- ✅ Fetches Twitch streamers' status in real-time  
- ✅ Displays streamer profile pictures, names, and stream titles  
- ✅ Shows the game being played (if online)  
- ✅ Provides quick links to Twitch channels  
- ✅ Includes filter buttons for easy navigation  

---

## **Project Structure**
📂 Twitch Stream Status ├── 📜 index.html # Main HTML structure ├── 📜 style.css # CSS for styling (embedded in HTML) ├── 📜 script.js # JavaScript logic (embedded in HTML)


---

## **Technologies Used**
- **HTML5** - For the structure of the webpage  
- **CSS3** - For styling and UI design  
- **JavaScript (ES6)** - For fetching and updating Twitch data  
- **Twitch API (FreeCodeCamp Proxy)** - For retrieving streamer details  

---

## **How It Works**
1. The application fetches data from the Twitch API for a list of predefined streamers.  
2. It checks whether each streamer is online or offline.  
3. The UI displays the streamer's profile picture, name, and stream details.  
4. Users can filter the list to show only online or offline streamers.  
5. Clicking on a streamer’s name opens their Twitch channel in a new tab.  

---

## **Usage Instructions**
### **1. Open the Application**
- Simply open `index.html` in a browser to view the Twitch Stream Status page.

### **2. Filtering Streamers**
- Click on the **"All"** button to see all streamers.  
- Click on the **"Online"** button to see only live streamers.  
- Click on the **"Offline"** button to see only offline streamers.  

---

## **Code Breakdown**
### **Fetching Streamer Data**
The following JavaScript function retrieves streamer details from the Twitch API:

```javascript
async function fetchStreamerData(username) {
    try {
        const channelRes = await fetch(`${API_BASE}/channels/${username}`);
        if (!channelRes.ok) return { status: 'closed', username };

        const channelData = await channelRes.json();
        const streamRes = await fetch(`${API_BASE}/streams/${username}`);
        const streamData = await streamRes.json();

        return {
            username,
            name: channelData.display_name,
            logo: channelData.logo || 'https://via.placeholder.com/50',
            url: channelData.url,
            status: streamData.stream ? 'online' : 'offline',
            game: streamData.stream?.game,
            title: streamData.stream?.channel.status
        };
    } catch (error) {
        console.error(`Error fetching data for ${username}:`, error);
        return { status: 'error', username };
    }
}
function createStreamerElement(streamer) {
    const div = document.createElement('div');
    div.className = `streamer ${streamer.status}`;

    div.innerHTML = `
        <a href="${streamer.url}" target="_blank">
            <img class="logo" src="${streamer.logo}" alt="${streamer.name}">
            <div class="stream-info">
                <div>
                    <i class="fas fa-circle status-icon"></i>
                    ${streamer.name}
                </div>
                ${streamer.status === 'online' ? 
                    `<div>${streamer.title}</div><span class="game">Playing: ${streamer.game}</span>` : 
                    '<div>Offline</div>'}
            </div>
        </a>
    `;
    return div;
}
