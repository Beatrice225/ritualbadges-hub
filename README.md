<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ritual Testnet Builder Hub & Badges</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/ethers/6.7.0/ethers.umd.min.js"></script>
    <style>
        :root {
            --bg-dark: #0b0f19;
            --card-bg: #151d30;
            --accent-blue: #3b82f6;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
        }
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            background: var(--bg-dark); 
            color: var(--text-main); 
            padding: 40px 20px; 
            margin: 0; 
        }
        .container { 
            max-width: 850px; 
            margin: auto; 
            background: var(--card-bg); 
            padding: 35px; 
            border-radius: 16px; 
            box-shadow: 0 10px 25px rgba(0,0,0,0.5); 
            border: 1px solid #222f4c;
        }
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #222f4c;
            padding-bottom: 20px;
            margin-bottom: 30px;
        }
        h1 { margin: 0; font-size: 24px; letter-spacing: -0.5px; }
        h2 { font-size: 20px; color: #fff; margin-top: 0; }
        .btn { 
            background: var(--accent-blue); 
            color: white; 
            border: none; 
            padding: 12px 24px; 
            border-radius: 8px; 
            cursor: pointer; 
            font-weight: 600; 
            transition: all 0.2s ease;
        }
        .btn:hover { background: #2563eb; transform: translateY(-1px); }
        .btn:disabled { background: #475569; cursor: not-allowed; transform: none; opacity: 0.6; }
        .info-box {
            background: #1e293b;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 25px;
            font-size: 14px;
            color: var(--text-muted);
            border-left: 4px solid var(--accent-blue);
        }
        .badge-grid { 
            display: grid; 
            grid-template-columns: repeat(auto-fit, minmax(160px, 1fr)); 
            gap: 15px; 
            margin: 20px 0; 
        }
        .badge { 
            padding: 20px 15px; 
            border-radius: 12px; 
            font-weight: bold; 
            text-align: center;
            opacity: 0.25; 
            background: #1e293b; 
            border: 2px solid transparent;
            transition: all 0.3s ease;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.2);
        }
        .active-badge { 
            opacity: 1 !important; 
            border-color: #fff; 
            box-shadow: 0 0 15px rgba(255,255,255,0.2);
        }
        .Bronze { background: linear-gradient(135deg, #78350f, #b45309); color: #fef3c7; }
        .Silver { background: linear-gradient(135deg, #334155, #64748b); color: #f1f5f9; }
        .Gold { background: linear-gradient(135deg, #713f12, #eab308); color: #fef9c3; }
        .Diamond { background: linear-gradient(135deg, #164e63, #06b6d4); color: #ecfeff; }
        
        .section {
            background: #101726;
            padding: 25px;
            border-radius: 12px;
            margin-bottom: 25px;
            border: 1px solid #1e293b;
        }
        input, textarea { 
            width: 100%; 
            padding: 12px; 
            margin: 10px 0 20px 0; 
            border-radius: 8px; 
            border: 1px solid #334155; 
            background: #1e293b; 
            color: white; 
            box-sizing: border-box;
            font-size: 14px;
        }
        input:focus, textarea:focus { border-color: var(--accent-blue); outline: none; }
        label { font-size: 14px; font-weight: 500; color: var(--text-muted); }
        
        .project-card { 
            background: #1e293b; 
            padding: 20px; 
            border-radius: 10px; 
            margin-top: 15px; 
            border: 1px solid #334155;
        }
        .project-card h3 { margin: 0 0 10px 0; display: flex; justify-content: space-between; font-size: 18px; }
        .project-tag { background: #334155; padding: 4px 10px; border-radius: 6px; font-size: 12px; color: var(--accent-blue); }
        .project-status { font-size: 12px; color: #10b981; background: rgba(16, 185, 129, 0.1); padding: 2px 8px; border-radius: 4px; }
    </style>
</head>
<body>

    <div class="container">
        <header>
            <h1>Ritual Ecosystem Hub</h1>
            <button id="connectBtn" class="btn">Connect Wallet</button>
        </header>

        <div id="walletDisplaySection" style="display: none;">
            <div class="info-box">
                <div id="walletAddress" style="font-weight: bold; color: #fff;">Wallet: Not Connected</div>
                <div id="txCountDisplay" style="margin-top: 5px;">Transactions calculated: 0</div>
            </div>
        </div>

        <div class="section">
            <h2>1. Ritual Activity Badges</h2>
            <p style="color: var(--text-muted); font-size: 14px; margin-top: -10px;">
                Mint a badge matching your transaction footprint on the Ritual Network.
            </p>
            <div class="badge-grid">
                <div id="badge-1" class="badge Bronze">Bronze<br><span style="font-size:11px; font-weight:normal;">&lt; 100 TXs</span></div>
                <div id="badge-2" class="badge Silver">Silver<br><span style="font-size:11px; font-weight:normal;">&gt; 100 TXs</span></div>
                <div id="badge-3" class="badge Gold">Gold<br><span style="font-size:11px; font-weight:normal;">&gt; 200 TXs</span></div>
                <div id="badge-4" class="badge Diamond">Diamond<br><span style="font-size:11px; font-weight:normal;">&gt; 500 TXs</span></div>
            </div>
            <button id="claimBtn" class="btn" style="display: none; width: 100%;">Analyze Footprint to Mint Badge</button>
        </div>

        <div class="section">
            <h2>2. Builder Registry</h2>
            <p style="color: var(--text-muted); font-size: 14px; margin-top: -10px;">
                Submit details regarding what you are building or have deployed on top of Ritual Chain.
            </p>
            
            <label for="projName">Project Name</label>
            <input type="text" id="projName" placeholder="e.g., Ritual Autonomous Agent Hub">

            <label for="projType">Project Type</label>
            <input type="text" id="projType" placeholder="e.g., AI Agent, Infrastructure, DeFi, NFT">

            <label for="projDesc">What are you building?</label>
            <textarea id="projDesc" rows="3" placeholder="Describe the utility and architecture of your project..."></textarea>

            <label for="projStatus">Project Status</label>
            <input type="text" id="projStatus" placeholder="e.g., Idea, Building, Mainnet Live">

            <button id="submitProjBtn" class="btn" style="width: 100%;">Register Project On-Chain</button>
        </div>

        <div class="section">
            <h2>3. Ritual Ecosystem Directory</h2>
            <div id="projectList">
                <p style="color: var(--text-muted); font-size: 14px;">Connect your wallet to fetch the builder registry map.</p>
            </div>
        </div>
    </div>

    <script>
        // Directly linked to your verified deployed contract address on Ritual Testnet
        const contractAddress = "0xea16F987B9e13ADcEc0766F5bFA6F5aDAA48cc50";
        
        const contractABI = [
            "function claimTxBadge(uint8 _tier) external",
            "function submitProject(string memory _name, string memory _description, string memory _projectType, string memory _status) external",
            "function getAllProjects() external view returns (tuple(string name, string description, string projectType, string status, address builder)[])",
            "function hasBadge(address user, uint8 tier) external view returns (bool)"
        ];

        let provider, signer, contract;
        let userAddress = "";
        let eligibleTier = 0; // 1 = Bronze, 2 = Silver, 3 = Gold, 4 = Diamond

        document.getElementById('connectBtn').addEventListener('click', connectWallet);
        document.getElementById('claimBtn').addEventListener('click', mintEligibleBadge);
        document.getElementById('submitProjBtn').addEventListener('click', submitProjectData);

        async function connectWallet() {
            if (!window.ethereum) {
                alert("MetaMask web3 provider not found! Please install MetaMask to use this application.");
                return;
            }

            try {
                provider = new ethers.BrowserProvider(window.ethereum);
                // Prompt user to link wallet accounts
                await provider.send("eth_requestAccounts", []);
                signer = await provider.getSigner();
                userAddress = await signer.getAddress();
                contract = new ethers.Contract(contractAddress, contractABI, signer);

                // Update UI layout states
                document.getElementById('walletAddress').innerText = Wallet: ${userAddress};
                document.getElementById('walletDisplaySection').style.display = 'block';
                document.getElementById('connectBtn').innerText = "Connected ✓";
                document.getElementById('connectBtn').disabled = true;

                await calculateActivityMetrics();
                await fetchEcosystemRegistry();
            } catch (error) {
                console.error("Wallet connection failure:", error);
                alert("Connection failed. Make sure you accept the connection request.");
            }
        }

        async function calculateActivityMetrics() {
            try {
                // Fetch nonce count from the active chain network environment
                const txCount = await provider.getTransactionCount(userAddress);
                document.getElementById('txCountDisplay').innerText = Total Outbound Transactions on Ritual: ${txCount};

                // Process tier assignment ranges
                if (txCount > 500) { eligibleTier = 4; }
                else if (txCount > 200) { eligibleTier = 3; }
                else if (txCount > 100) { eligibleTier = 2; }
                else { eligibleTier = 1; }

                // Reset and light up target visual node item
                for (let i = 1; i <= 4; i++) {
                    document.getElementById(badge-${i}).classList.remove('active-badge');
                }
                document.getElementById(badge-${eligibleTier}).classList.add('active-badge');

                // Cross reference smart contract mappings to see if tier was claimed previously
                const alreadyClaimed = await contract.hasBadge(userAddress, eligibleTier);
                const claimButton = document.getElementById('claimBtn');
                claimButton.style.display = 'block';

                if (alreadyClaimed) {
                    claimButton.innerText = "Badge Level Already Claimed ✓";
                    claimButton.disabled = true;
                } else {
                    const tierNames = ["", "Bronze", "Silver", "Gold", "Diamond"];
                    claimButton.innerText = Mint Your Verified ${tierNames[eligibleTier]} Badge;
                    claimButton.disabled = false;
                }
            } catch (err) {
                console.error("Error reading transaction volume footprints:", err);
            }
        }

        async function mintEligibleBadge() {
            const claimButton = document.getElementById('claimBtn');
            try {
                claimButton.innerText = "Sending Broadcast Request...";
                claimButton.disabled = true;

                const tx = await contract.claimTxBadge(eligibleTier);
                claimButton.innerText = "Mining Transaction Block...";
                
                await tx.wait();
                alert("Success! Your verification contribution badge has been stored on the Ritual Chain.");
                await calculateActivityMetrics();
            } catch (error) {
                console.error("Minting transaction failure:", error);
                alert("Transaction rejected or failed. Check explorer details.");
                await calculateActivityMetrics();
            }
        }

        async function submitProjectData() {
            const name = document.getElementById('projName').value.trim();
            const type = document.getElementById('projType').value.trim();
            const desc = document.getElementById('projDesc').value.trim();
            const status = document.getElementById('projStatus').value.trim();
            const submitBtn = document.getElementById('submitProjBtn');

            if (!userAddress) {
                return alert("Please connect your MetaMask wallet first!");
            }
            if (!name || !type || !desc || !status) {
                return alert("Please fulfill all parameters before submitting data mapping.");
            }

            try {
                submitBtn.innerText = "Transmitting to Ritual Registry...";
                submitBtn.disabled = true;

                const tx = await contract.submitProject(name, desc, type, status);
                submitBtn.innerText = "Waiting for Confirmation...";
                
                await tx.wait();
                alert("Project indexed perfectly into the global directory framework!");
                
                // Clear out form inputs
                document.getElementById('projName').value = "";
                document.getElementById('projType').value = "";
                document.getElementById('projDesc').value = "";
                document.getElementById('projStatus').value = "";

                await fetchEcosystemRegistry();
            } catch (error) {
                console.error("Failed to commit project structure state:", error);
                alert("Transaction execution dropped.");
            } finally {
                submitBtn.innerText = "Register Project On-Chain";
                submitBtn.disabled = false;
            }
        }

        async function fetchEcosystemRegistry() {
            const displayBox = document.getElementById('projectList');
            try {
                displayBox.innerHTML = "<p style='color: var(--text-muted);'>Syncing registry pipeline...</p>";
                const records = await contract.getAllProjects();

                if (records.length === 0) {
                    displayBox.innerHTML = "<p style='color: var(--text-muted); font-size: 14px;'>No projects registered yet. Be the first to build on Ritual!</p>";
                    return;
                }

                displayBox.innerHTML = "";
                // Loop backwards so newest submissions display on top
                for (let i = records.length - 1; i >= 0; i--) {
                    const project = records[i];
                    const card = document.createElement('div');
                    card.className = 'project-card';
                    card.innerHTML = `
                        <h3>
                            <span>${escapeHTML(project.name)}</span>
                            <span class="project-tag">${escapeHTML(project.projectType)}</span>
                        </h3>
                        <p style="margin: 0 0 15px 0; font-size: 14px; color: #cbd5e1; line-height: 1.5;">${escapeHTML(project.description)}</p>
                        <div style="display: flex; justify-content: space-between; align-items: center;">
                            <span class="project-status">${escapeHTML(project.status)}</span>
                            <span style="font-size: 11px; color: var(--text-muted);">Builder: ${project.builder.substring(0,6)}...${project.builder.substring(38)}</span>
                        </div>
                    `;
                    displayBox.appendChild(card);
                }
            } catch (err) {
                console.error("Error reading repository mapping storage arrays:", err);
                displayBox.innerHTML = "<p style='color: #ef4444;'>Failed to parse on-chain directory lists.</p>";
            }
        }

        // Helper function to prevent basic cross-site script injections
        function escapeHTML(str) {
            return str.replace(/[&<>'"]/g, 
                tag => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', "'": '&#39;', '"': '&quot;' }[tag] || tag)
            );
        }
    </script>
</body>
</html>
