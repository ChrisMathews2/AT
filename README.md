# AT
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AT-AC Transition Programme Workspace</title>
    
    <style>
      #atWorkspaceWrapper{background:transparent;padding:24px 0}
      #atWorkspace{max-width:1400px;margin:0 auto;font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,sans-serif;color:#1f2937;line-height:1.4}
      #atWorkspace *{box-sizing:border-box}
      
      /* Sync status indicator */
      #atWorkspace .sync-status{position:fixed;top:10px;right:10px;padding:6px 12px;border-radius:20px;font-size:12px;z-index:1000;display:flex;align-items:center;gap:6px;background:#dcfce7;color:#15803d}
      #atWorkspace .sync-dot{width:8px;height:8px;border-radius:50%;background:currentColor;animation:pulse 2s infinite}
      @keyframes pulse{0%{opacity:1}50%{opacity:0.5}100%{opacity:1}}
      
      /* header & nav */
      #atWorkspace header{background:linear-gradient(135deg, #1e3a8a 0%, #dc2626 100%);border-radius:12px;padding:20px 30px;display:flex;align-items:center;gap:10px;box-shadow:0 4px 12px rgba(0,0,0,.1);justify-content:space-between;color:white}
      #atWorkspace header h1{margin:0;font-size:1.8rem;font-weight:700}
      #atWorkspace header .subtitle{font-size:0.9rem;opacity:0.9;margin-top:4px}
      #atWorkspace header .btn{background:rgba(255,255,255,0.2);color:white;border:1px solid rgba(255,255,255,0.3)}
      #atWorkspace header .btn:hover{background:rgba(255,255,255,0.3)}
      #atWorkspace .vault-btn{position:relative;padding-right:40px}
      #atWorkspace .vault-badge{position:absolute;top:-8px;right:-8px;background:#dc2626;color:#fff;font-size:0.75rem;font-weight:700;padding:2px 8px;border-radius:20px;min-width:24px;text-align:center}
      
      #atWorkspace nav{background:#fff;border-radius:12px;margin:22px 0;display:flex;justify-content:space-around;box-shadow:0 2px 6px rgba(0,0,0,.06)}
      #atWorkspace .tab{flex:1;text-align:center;padding:14px 8px;cursor:pointer;font-size:1rem;border-bottom:4px solid transparent;transition:all 0.2s;position:relative}
      #atWorkspace .tab:hover{background:#f8fafc}
      #atWorkspace .tab.active{border-bottom-color:#2563eb;background:#f9fafb;font-weight:700}
      #atWorkspace .tab.active::after{content:'';position:absolute;bottom:-4px;left:0;right:0;height:4px;background:linear-gradient(90deg, #3b82f6 0%, #dc2626 100%)}
      
      /* panels */
      #atWorkspace .panel{display:none}
      #atWorkspace .panel.active{display:block}
      #atWorkspace .panel-header{display:flex;justify-content:space-between;align-items:center;margin:28px 0 18px}
      #atWorkspace .panel-header h2{margin:0;color:#1f2937;font-size:1.5rem}
      #atWorkspace .add-btn{background:linear-gradient(135deg, #10b981 0%, #059669 100%);color:#fff;border:none;padding:10px 18px;border-radius:8px;font-size:.9rem;cursor:pointer;transition:all 0.2s;box-shadow:0 2px 6px rgba(16, 185, 129, 0.2)}
      #atWorkspace .add-btn:hover{transform:translateY(-1px);box-shadow:0 4px 12px rgba(16, 185, 129, 0.3)}
      #atWorkspace .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(380px,1fr));gap:28px}
      
      /* card - default (white) theme */
      #atWorkspace .card{background:#fff;border-radius:14px;padding:24px;padding-top:50px;display:flex;flex-direction:column;gap:12px;position:relative;box-shadow:0 2px 6px rgba(0,0,0,.06);transition:transform 0.2s,box-shadow 0.2s;border:1px solid #e5e7eb;min-height:200px}
      #atWorkspace .card:hover{transform:translateY(-2px);box-shadow:0 4px 12px rgba(0,0,0,.1);border-color:#ddd6fe}
      #atWorkspace .card h3{margin:0;font-size:1.1rem;color:#1f2937;word-wrap:break-word;padding-right:90px}
      #atWorkspace .card p{margin:0;color:#6b7280;font-size:.9rem;line-height:1.5}
      
      /* card - dark theme */
      #atWorkspace .card.dark-theme{background:#1e293b;color:#e5e7eb;border:1px solid #334155}
      #atWorkspace .card.dark-theme h3{color:#f3f4f6}
      #atWorkspace .card.dark-theme p{color:#cbd5e1}
      #atWorkspace .card.dark-theme .link-list a{color:#60a5fa}
      #atWorkspace .card.dark-theme .file-item{background:#0f172a;color:#e5e7eb;border:1px solid #334155}
      #atWorkspace .card.dark-theme .file-item:hover{background:#334155}
      #atWorkspace .card.dark-theme .vault-status{background:#1e3a8a;color:#93c5fd}
      
      /* Priority indicators */
      #atWorkspace .priority-badge{position:absolute;top:12px;left:12px;padding:6px 12px;border-radius:6px;font-size:0.75rem;font-weight:600;z-index:10}
      #atWorkspace .priority-high{background:#fee2e2;color:#dc2626}
      #atWorkspace .priority-medium{background:#fef3c7;color:#92400e}
      #atWorkspace .priority-low{background:#dcfce7;color:#15803d}
      #atWorkspace .card.dark-theme .priority-high{background:#dc2626;color:#fff}
      #atWorkspace .card.dark-theme .priority-medium{background:#f59e0b;color:#fff}
      #atWorkspace .card.dark-theme .priority-low{background:#10b981;color:#fff}
      
      /* theme toggle button */
      #atWorkspace .theme-toggle{position:absolute;top:12px;right:50px;background:#f3f4f6;border:none;width:36px;height:36px;border-radius:8px;cursor:pointer;font-size:18px;transition:all 0.2s;display:flex;align-items:center;justify-content:center;z-index:10}
      #atWorkspace .theme-toggle:hover{background:#e5e7eb}
      #atWorkspace .card.dark-theme .theme-toggle{background:#475569;color:#fff}
      #atWorkspace .card.dark-theme .theme-toggle:hover{background:#64748b}
      
      #atWorkspace .del{position:absolute;top:12px;right:12px;background:#fee2e2;border:none;width:36px;height:36px;border-radius:8px;color:#dc2626;cursor:pointer;font-size:1.1rem;transition:background 0.2s;display:flex;align-items:center;justify-content:center;z-index:10}
      #atWorkspace .del:hover{background:#fecaca}
      #atWorkspace .card.dark-theme .del{background:#dc2626;color:#fff}
      #atWorkspace .card.dark-theme .del:hover{background:#ef4444}
      
      #atWorkspace .actions{display:flex;flex-wrap:wrap;gap:8px;margin-top:auto}
      #atWorkspace .pill{border:none;border-radius:6px;padding:8px 14px;font-size:.8rem;cursor:pointer;transition:all 0.2s;font-weight:500}
      #atWorkspace .blue{background:#e0e7ff;color:#3730a3}
      #atWorkspace .blue:hover{background:#c7d2fe}
      #atWorkspace .green{background:#dcfce7;color:#15803d}
      #atWorkspace .green:hover{background:#bbf7d0}
      #atWorkspace .yellow{background:#fef9c3;color:#92400e}
      #atWorkspace .yellow:hover{background:#fef3c7}
      #atWorkspace .gray{background:#f3f4f6;color:#374151}
      #atWorkspace .gray:hover{background:#e5e7eb}
      #atWorkspace .red{background:#fee2e2;color:#dc2626}
      #atWorkspace .red:hover{background:#fecaca}
      #atWorkspace .purple{background:#ede9fe;color:#6b21a8}
      #atWorkspace .purple:hover{background:#ddd6fe}
      
      /* Dark theme pills */
      #atWorkspace .card.dark-theme .blue{background:#3730a3;color:#e0e7ff}
      #atWorkspace .card.dark-theme .blue:hover{background:#4c1d95}
      #atWorkspace .card.dark-theme .green{background:#15803d;color:#dcfce7}
      #atWorkspace .card.dark-theme .green:hover{background:#166534}
      #atWorkspace .card.dark-theme .yellow{background:#92400e;color:#fef3c7}
      #atWorkspace .card.dark-theme .yellow:hover{background:#78350f}
      #atWorkspace .card.dark-theme .gray{background:#475569;color:#e5e7eb}
      #atWorkspace .card.dark-theme .gray:hover{background:#64748b}
      #atWorkspace .card.dark-theme .red{background:#dc2626;color:#fff}
      #atWorkspace .card.dark-theme .red:hover{background:#ef4444}
      #atWorkspace .card.dark-theme .purple{background:#6b21a8;color:#ede9fe}
      #atWorkspace .card.dark-theme .purple:hover{background:#7c3aed}
      
      #atWorkspace .pill:disabled{opacity:.5;cursor:not-allowed}
      #atWorkspace .link-list{margin-top:10px;padding:10px;background:#f9fafb;border-radius:8px}
      #atWorkspace .card.dark-theme .link-list{background:rgba(0,0,0,0.3)}
      #atWorkspace .link-list a{display:block;color:#2563eb;font-size:.85rem;text-decoration:none;word-break:break-all;margin-bottom:6px;padding:4px 0}
      #atWorkspace .link-list a:hover{text-decoration:underline}
      #atWorkspace .link-list a:last-child{margin-bottom:0}
      
      /* File list styling */
      #atWorkspace .file-list{margin-top:12px;border-top:1px solid #e5e7eb;padding-top:12px}
      #atWorkspace .card.dark-theme .file-list{border-top-color:#475569}
      #atWorkspace .file-item{display:flex;align-items:center;justify-content:space-between;padding:10px;margin-bottom:8px;background:#f9fafb;border-radius:8px;font-size:.85rem;gap:10px}
      #atWorkspace .file-item:hover{background:#f3f4f6}
      #atWorkspace .file-info{display:flex;align-items:center;gap:8px;flex:1;min-width:0}
      #atWorkspace .file-info span{overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
      #atWorkspace .file-actions{display:flex;gap:6px;flex-shrink:0}
      #atWorkspace .vault-status{font-size:.7rem;padding:3px 8px;border-radius:4px;background:#e0e7ff;color:#3730a3;white-space:nowrap}
      
      /* Status tags */
      #atWorkspace .status-tags{display:flex;gap:6px;margin-top:8px;flex-wrap:wrap}
      #atWorkspace .status-tag{font-size:0.75rem;padding:4px 10px;border-radius:6px;font-weight:600}
      #atWorkspace .tag-at{background:#fee2e2;color:#dc2626}
      #atWorkspace .tag-ac{background:#dbeafe;color:#1e40af}
      #atWorkspace .tag-integrated{background:#ede9fe;color:#6b21a8}
      #atWorkspace .card.dark-theme .tag-at{background:#dc2626;color:#fff}
      #atWorkspace .card.dark-theme .tag-ac{background:#2563eb;color:#fff}
      #atWorkspace .card.dark-theme .tag-integrated{background:#7c3aed;color:#fff}
      
      /* Modal styles */
      #atWorkspace .overlay{position:fixed;inset:0;background:rgba(0,0,0,.45);display:flex;align-items:center;justify-content:center;visibility:hidden;opacity:0;transition:.2s;z-index:1000}
      #atWorkspace .overlay.show{visibility:visible;opacity:1}
      #atWorkspace #vaultOverlay{z-index:1100}
      #atWorkspace .modal{background:#fff;border-radius:12px;width:90%;max-width:600px;padding:26px;display:flex;flex-direction:column;gap:14px;max-height:92vh;overflow:auto}
      #atWorkspace .modal h3{margin:0;color:#1f2937}
      #atWorkspace .modal input,#atWorkspace .modal textarea,#atWorkspace .modal select{border:1px solid #d1d5db;border-radius:6px;padding:8px 12px;font-size:.9rem;width:100%}
      #atWorkspace .modal input:focus,#atWorkspace .modal textarea:focus,#atWorkspace .modal select:focus{outline:none;border-color:#2563eb;box-shadow:0 0 0 2px rgba(37,99,235,.1)}
      #atWorkspace .btn{background:#f3f4f6;color:#374151;border:none;padding:8px 16px;border-radius:6px;cursor:pointer;font-size:.9rem;transition:background 0.2s}
      #atWorkspace .btn:hover{background:#e5e7eb}
      #atWorkspace .btn:disabled{opacity:0.5;cursor:not-allowed}
      #atWorkspace .btn:disabled:hover{background:#f3f4f6}
      
      /* Priority selector */
      #atWorkspace .priority-selector{display:flex;gap:8px;margin-top:8px}
      #atWorkspace .priority-option{flex:1;padding:8px;border:2px solid #e5e7eb;border-radius:6px;text-align:center;cursor:pointer;transition:all 0.2s}
      #atWorkspace .priority-option:hover{border-color:#ddd6fe}
      #atWorkspace .priority-option.selected{border-color:#2563eb;background:#eff6ff}
      
      /* Notification */
      #atWorkspace .notification{position:fixed;top:60px;right:20px;background:#10b981;color:#fff;padding:12px 20px;border-radius:8px;box-shadow:0 4px 12px rgba(0,0,0,.15);opacity:0;transform:translateY(-20px);transition:all 0.3s;z-index:1200}
      #atWorkspace .notification.show{opacity:1;transform:translateY(0)}
      
      /* Quick stats bar */
      #atWorkspace .stats-bar{background:#f9fafb;border-radius:8px;padding:16px;margin-bottom:20px;display:grid;grid-template-columns:repeat(auto-fit, minmax(150px, 1fr));gap:16px}
      #atWorkspace .stat-item{text-align:center}
      #atWorkspace .stat-value{font-size:1.8rem;font-weight:700;color:#1f2937}
      #atWorkspace .stat-label{font-size:0.8rem;color:#6b7280}
      
      /* Vault Modal */
      #atWorkspace .vault-modal{background:#fff;border-radius:16px;width:90%;max-width:900px;max-height:90vh;display:flex;flex-direction:column;box-shadow:0 20px 50px rgba(0,0,0,.2)}
      #atWorkspace .vault-header{background:linear-gradient(135deg, #1e3a8a 0%, #dc2626 100%);color:#fff;padding:24px;border-radius:16px 16px 0 0;display:flex;justify-content:space-between;align-items:center}
      #atWorkspace .vault-header h2{margin:0;font-size:1.5rem}
      #atWorkspace .vault-close{background:rgba(255,255,255,0.2);border:none;color:#fff;width:36px;height:36px;border-radius:8px;font-size:24px;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:background 0.2s}
      #atWorkspace .vault-close:hover{background:rgba(255,255,255,0.3)}
      
      #atWorkspace .vault-controls{padding:20px;border-bottom:1px solid #e5e7eb;display:flex;gap:16px;flex-wrap:wrap;align-items:center}
      #atWorkspace .vault-search{flex:1;min-width:250px;position:relative}
      #atWorkspace .vault-search input{width:100%;padding:10px 40px 10px 16px;border:1px solid #d1d5db;border-radius:8px;font-size:0.9rem}
      #atWorkspace .vault-search input:focus{outline:none;border-color:#2563eb;box-shadow:0 0 0 2px rgba(37,99,235,.1)}
      #atWorkspace .search-clear{position:absolute;right:8px;top:50%;transform:translateY(-50%);background:#dc2626;color:#fff;border:none;width:28px;height:28px;border-radius:6px;cursor:pointer;font-size:18px;display:flex;align-items:center;justify-content:center;transition:all 0.2s}
      #atWorkspace .search-clear:hover{background:#ef4444}
      #atWorkspace .vault-filters{display:flex;gap:10px}
      #atWorkspace .vault-filters select{padding:10px 16px;border:1px solid #d1d5db;border-radius:8px;font-size:0.9rem;background:#fff;cursor:pointer}
      #atWorkspace .vault-filters select:focus{outline:none;border-color:#2563eb}
      
      #atWorkspace .vault-stats{padding:20px;background:#f9fafb;display:grid;grid-template-columns:repeat(auto-fit, minmax(120px, 1fr));gap:20px}
      #atWorkspace .vault-stat{text-align:center}
      #atWorkspace .vault-stat-value{display:block;font-size:1.5rem;font-weight:700;color:#1f2937}
      #atWorkspace .vault-stat-label{display:block;font-size:0.8rem;color:#6b7280;margin-top:4px}
      
      #atWorkspace .vault-content{flex:1;overflow-y:auto;padding:20px;min-height:300px}
      #atWorkspace .vault-empty{text-align:center;padding:60px 20px;color:#6b7280}
      #atWorkspace .vault-empty-icon{font-size:3rem;opacity:0.3;margin-bottom:16px}
      
      #atWorkspace .vault-item{background:#fff;border:1px solid #e5e7eb;border-radius:12px;padding:16px;margin-bottom:12px;display:flex;align-items:center;gap:16px;transition:all 0.2s;cursor:pointer}
      #atWorkspace .vault-item:hover{border-color:#ddd6fe;box-shadow:0 2px 8px rgba(0,0,0,.06);transform:translateY(-1px)}
      #atWorkspace .vault-item-icon{width:48px;height:48px;background:#f3f4f6;border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:24px;flex-shrink:0}
      #atWorkspace .vault-item-info{flex:1;min-width:0}
      #atWorkspace .vault-item-name{font-weight:600;color:#1f2937;margin-bottom:4px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
      #atWorkspace .vault-item-meta{font-size:0.8rem;color:#6b7280;display:flex;gap:16px;flex-wrap:wrap}
      #atWorkspace .vault-item-actions{display:flex;gap:8px;flex-shrink:0}
      
      #atWorkspace .vault-footer{padding:20px;border-top:1px solid #e5e7eb;display:flex;gap:12px;justify-content:space-between}
      
      /* File type colors */
      #atWorkspace .vault-item-icon.pdf{background:#fee2e2;color:#dc2626}
      #atWorkspace .vault-item-icon.doc{background:#dbeafe;color:#2563eb}
      #atWorkspace .vault-item-icon.xls{background:#d1fae5;color:#10b981}
      #atWorkspace .vault-item-icon.img{background:#fce7f3;color:#ec4899}
      #atWorkspace .vault-item-icon.other{background:#f3f4f6;color:#6b7280}
      
      /* Responsive design */
      @media (max-width: 768px) {
          #atWorkspace .vault-modal{width:100%;height:100vh;max-height:100vh;border-radius:0}
          #atWorkspace .vault-header{border-radius:0}
          #atWorkspace .vault-controls{flex-direction:column;align-items:stretch}
          #atWorkspace .vault-search{min-width:auto}
          #atWorkspace .vault-filters{flex-direction:column;width:100%}
          #atWorkspace .vault-filters select{width:100%}
          #atWorkspace .vault-item{flex-wrap:wrap}
          #atWorkspace .vault-item-info{width:100%}
          #atWorkspace .vault-item-actions{width:100%;margin-top:12px;justify-content:stretch}
          #atWorkspace .vault-item-actions .pill{flex:1;text-align:center}
          #atWorkspace .vault-footer{flex-direction:column}
          #atWorkspace .vault-footer button{width:100%}
      }
    </style>
</head>
<body>
    <div id="atWorkspaceWrapper">
        <!-- Sync Status Indicator -->
        <div class="sync-status" id="syncStatus">
            <span class="sync-dot"></span>
            <span id="syncText">Local Mode</span>
        </div>

        <div id="atWorkspace">
            <header>
                <div>
                    <h1>🚌 AT-AC Transition Programme</h1>
                    <div class="subtitle">Collaborative Workspace & Document Hub</div>
                </div>
                <button class="btn vault-btn" id="openVaultTop" onclick="openVault()">
                    📂 Document Vault
                    <span class="vault-badge" id="vaultBadge">0</span>
                </button>
            </header>
            
            <!-- Quick Stats -->
            <div class="stats-bar" id="statsBar">
                <div class="stat-item">
                    <div class="stat-value" id="totalItems">0</div>
                    <div class="stat-label">Total Items</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="activeWorkstreams">0</div>
                    <div class="stat-label">Active Workstreams</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="upcomingMilestones">0</div>
                    <div class="stat-label">Upcoming Milestones</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="totalDocuments">0</div>
                    <div class="stat-label">Documents</div>
                </div>
            </div>
            
            <nav>
                <div class="tab" data-type="overview">📊 Programme Overview</div>
                <div class="tab" data-type="workstreams">🔄 Workstreams</div>
                <div class="tab" data-type="milestones">🎯 Milestones</div>
                <div class="tab" data-type="resources">📚 Resources</div>
            </nav>
            
            <main></main>

            <!-- Item Modal -->
            <div class="overlay" id="modalOv">
                <div class="modal">
                    <h3 id="modalHead">New Item</h3>
                    <input id="titleInput" placeholder="Title" required>
                    <textarea id="descInput" placeholder="Description" rows="3"></textarea>
                    
                    <!-- Priority selector -->
                    <div>
                        <label style="font-size:.9rem;margin-bottom:6px;display:block">Priority Level</label>
                        <div class="priority-selector">
                            <div class="priority-option" data-priority="high">🔴 High</div>
                            <div class="priority-option selected" data-priority="medium">🟡 Medium</div>
                            <div class="priority-option" data-priority="low">🟢 Low</div>
                        </div>
                    </div>
                    
                    <!-- Organization tags -->
                    <div>
                        <label style="font-size:.9rem;margin-bottom:6px;display:block">Organization</label>
                        <select id="orgSelect">
                            <option value="">Select organization...</option>
                            <option value="at">Auckland Transport</option>
                            <option value="ac">Auckland Council</option>
                            <option value="integrated">Integrated/Joint</option>
                        </select>
                    </div>
                    
                    <!-- Due date for milestones -->
                    <div id="dueDateGroup" style="display:none">
                        <label style="font-size:.9rem;margin-bottom:6px;display:block">Due Date</label>
                        <input type="date" id="dueDateInput">
                    </div>
                    
                    <div class="link-input-group" style="display:flex;gap:6px">
                        <input id="linkInput" placeholder="Add URL" style="flex:1">
                        <button class="btn" id="addLink">＋</button>
                    </div>
                    <div id="linkPreview" class="link-preview" style="font-size:.75rem;color:#6b7280"></div>
                    
                    <input id="driveInput" placeholder="Google Drive folder URL (optional)">
                    
                    <div>
                        <label style="font-size:.9rem;margin-bottom:6px;display:block">📎 Upload Documents</label>
                        <input type="file" id="fileInput" multiple accept=".pdf,.doc,.docx,.txt,.xlsx,.pptx,.jpg,.png">
                        <div id="filePreview" class="file-upload-preview"></div>
                    </div>
                    
                    <div style="display:flex;gap:8px;justify-content:flex-end">
                        <button class="btn" id="cancelBtn">Cancel</button>
                        <button class="btn" style="background:#2563eb;color:#fff" id="saveBtn">Save</button>
                    </div>
                </div>
            </div>

            <!-- Notification -->
            <div class="notification" id="notification"></div>
            
            <!-- Vault Modal -->
            <div class="overlay" id="vaultOverlay" data-testid="vault-overlay">
                <div class="vault-modal">
                    <div class="vault-header">
                        <h2>📂 Document Vault</h2>
                        <button class="vault-close" onclick="closeVault()">×</button>
                    </div>
                    
                    <div class="vault-controls">
                        <div class="vault-search">
                            <input type="text" id="vaultSearch" placeholder="Search documents..." onkeyup="searchVault()">
                            <button class="search-clear" id="searchClear" onclick="clearSearch()" style="display:none">×</button>
                        </div>
                        <div class="vault-filters">
                            <select id="vaultFilter" onchange="filterVault()">
                                <option value="all">All Documents</option>
                                <option value="pdf">PDF Files</option>
                                <option value="doc">Word Documents</option>
                                <option value="xls">Spreadsheets</option>
                                <option value="img">Images</option>
                                <option value="other">Other</option>
                            </select>
                            <select id="vaultSort" onchange="sortVault()">
                                <option value="date-desc">Newest First</option>
                                <option value="date-asc">Oldest First</option>
                                <option value="name-asc">Name (A-Z)</option>
                                <option value="name-desc">Name (Z-A)</option>
                                <option value="size-desc">Largest First</option>
                                <option value="size-asc">Smallest First</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="vault-stats">
                        <div class="vault-stat">
                            <span class="vault-stat-value" id="vaultTotal">0</span>
                            <span class="vault-stat-label">Total Files</span>
                        </div>
                        <div class="vault-stat">
                            <span class="vault-stat-value" id="vaultSize">0 MB</span>
                            <span class="vault-stat-label">Total Size</span>
                        </div>
                        <div class="vault-stat">
                            <span class="vault-stat-value" id="vaultRecent">0</span>
                            <span class="vault-stat-label">Added This Week</span>
                        </div>
                    </div>
                    
                    <div class="vault-content" id="vaultContent">
                        <div class="vault-empty">
                            <div class="vault-empty-icon">⏳</div>
                            <p>Loading vault...</p>
                        </div>
                    </div>
                    
                    <div class="vault-footer">
                        <button class="btn" style="background:#6b7280;color:#fff" onclick="exportVaultList()">📥 Export List</button>
                        <button class="btn" style="background:#dc2626;color:#fff" onclick="clearVault()" id="clearVaultBtn">🗑️ Clear Vault</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
    (function() {
        'use strict';
        
        // Initialize workspace
        const $ = (s, p = document) => p.querySelector(s);
        const $$ = (s, p = document) => p.querySelectorAll(s);
        const workspace = $('#atWorkspace');
        
        if (!workspace) {
            console.error('Workspace element not found');
            return;
        }
        
        let main = $('main', workspace);
        if (!main) {
            main = document.createElement('main');
            workspace.appendChild(main);
        }

        // State management
        const TYPES = ['overview', 'workstreams', 'milestones', 'resources'];
        const DEF = {
            next: 1,
            view: 'overview',
            cards: Object.fromEntries(TYPES.map(t => [t, {}])),
            vault: [],
            themes: {}
        };
        
        let S = structuredClone(DEF);

        // Local storage functions
        function saveToLocal() {
            try {
                localStorage.setItem('atWorkspaceStore', JSON.stringify(S));
            } catch (e) {
                console.error('Local save error:', e);
                showNotification('Failed to save locally', 'error');
            }
        }

        function loadLocalData() {
            try {
                const localData = localStorage.getItem('atWorkspaceStore');
                if (localData) {
                    S = { ...DEF, ...JSON.parse(localData) };
                }
            } catch (e) {
                console.error('Local load error:', e);
                S = structuredClone(DEF);
            }
        }

        // Update stats
        function updateStats() {
            let totalItems = 0;
            let activeWorkstreams = 0;
            let upcomingMilestones = 0;
            let totalDocs = 0;

            TYPES.forEach(type => {
                const cards = Object.values(S.cards[type] || {});
                totalItems += cards.length;
                
                if (type === 'workstreams') {
                    activeWorkstreams = cards.filter(c => !c.completed).length;
                }
                
                if (type === 'milestones') {
                    const now = new Date();
                    const thirtyDays = new Date(now.getTime() + 30 * 24 * 60 * 60 * 1000);
                    upcomingMilestones = cards.filter(c => {
                        if (c.dueDate) {
                            const due = new Date(c.dueDate);
                            return due >= now && due <= thirtyDays;
                        }
                        return false;
                    }).length;
                }
                
                cards.forEach(card => {
                    totalDocs += (card.files || []).length;
                });
            });

            const updateStat = (id, value) => {
                const el = $('#' + id);
                if (el) el.textContent = value;
            };

            updateStat('totalItems', totalItems);
            updateStat('activeWorkstreams', activeWorkstreams);
            updateStat('upcomingMilestones', upcomingMilestones);
            updateStat('totalDocuments', totalDocs);
            
            // Update vault badge
            updateStat('vaultBadge', S.vault.length);
        }

        // Notification system
        const showNotification = (message, type = 'success', duration = 3000) => {
            const notif = $('#notification');
            if (!notif) return;
            
            notif.textContent = message;
            notif.style.background = type === 'error' ? '#ef4444' : '#10b981';
            notif.classList.add('show');
            setTimeout(() => notif.classList.remove('show'), duration);
        };

        // Build panels
        const panelInfo = {
            overview: { title: 'Programme Overview', icon: '📊' },
            workstreams: { title: 'Active Workstreams', icon: '🔄' },
            milestones: { title: 'Key Milestones', icon: '🎯' },
            resources: { title: 'Resources & Documents', icon: '📚' }
        };

        TYPES.forEach(t => {
            main.insertAdjacentHTML('beforeend', 
                `<section id='${t}' class='panel'>
                    <div class='panel-header'>
                        <h2>${panelInfo[t].icon} ${panelInfo[t].title}</h2>
                        <button class='add-btn' data-type='${t}'>+ Add ${t.slice(0, -1)}</button>
                    </div>
                    <div class='grid' id='grid_${t}'></div>
                </section>`
            );
        });

        // Tab switching
        $$('.tab', workspace).forEach(tab => {
            tab.onclick = () => {
                $$('.tab', workspace).forEach(x => x.classList.toggle('active', x === tab));
                TYPES.forEach(t => $('#' + t).classList.toggle('active', t === tab.dataset.type));
                S.view = tab.dataset.type;
                saveToLocal();
                
                // Show/hide due date for milestones
                if (tab.dataset.type === 'milestones') {
                    $('#dueDateGroup').style.display = 'block';
                } else {
                    $('#dueDateGroup').style.display = 'none';
                }
            };
        });

        // Modal functionality
        const ov = $('#modalOv');
        const mH = $('#modalHead');
        const mT = $('#titleInput');
        const mD = $('#descInput');
        const mLinkInp = $('#linkInput');
        const linkPrev = $('#linkPreview');
        const addLinkBtn = $('#addLink');
        const mDrive = $('#driveInput');
        const mFile = $('#fileInput');
        const filePrev = $('#filePreview');
        const saveBtn = $('#saveBtn');
        const cancelBtn = $('#cancelBtn');
        const orgSelect = $('#orgSelect');
        const dueDateInput = $('#dueDateInput');

        let linksTemp = [];
        let filesTemp = [];
        let curType = null;
        let editId = null;
        let selectedPriority = 'medium';

        // Priority selector
        $$('.priority-option').forEach(opt => {
            opt.onclick = () => {
                $$('.priority-option').forEach(o => o.classList.remove('selected'));
                opt.classList.add('selected');
                selectedPriority = opt.dataset.priority;
            };
        });

        const drawLinks = () => {
            linkPrev.innerHTML = linksTemp.length ? 
                linksTemp.map((l, i) => `<span style="display:block;margin:2px 0">${l} <b data-rm='${i}' style='cursor:pointer;color:#dc2626;margin-left:8px'>×</b></span>`).join('') :
                '<em style="color:#9ca3af">No links yet</em>';
        };

        const drawFiles = () => {
            filePrev.innerHTML = filesTemp.length ?
                '<div>' + filesTemp.map((f, i) => 
                    `<div class="file-upload-item">
                        <span>📎 ${f.name} (${(f.size / 1024).toFixed(1)} KB)</span>
                        <b data-rmfile='${i}' style='cursor:pointer;color:#dc2626'>×</b>
                    </div>`
                ).join('') + '</div>' :
                '<em style="color:#9ca3af">No files selected</em>';
        };

        // Add link functionality
        addLinkBtn.onclick = () => {
            const url = mLinkInp.value.trim();
            if (!url) return;
            try {
                new URL(url);
                linksTemp.push(url);
                mLinkInp.value = '';
                drawLinks();
            } catch {
                alert('Invalid URL');
            }
        };

        // Remove link functionality
        linkPrev.onclick = e => {
            if (e.target.dataset.rm !== undefined) {
                linksTemp.splice(+e.target.dataset.rm, 1);
                drawLinks();
            }
        };

        // Remove file functionality
        filePrev.onclick = e => {
            if (e.target.dataset.rmfile !== undefined) {
                filesTemp.splice(+e.target.dataset.rmfile, 1);
                drawFiles();
            }
        };

        // File input handling
        mFile.onchange = e => {
            filesTemp = [...e.target.files];
            drawFiles();
        };

        // Modal open/close
        const openModal = (type, id = null) => {
            curType = type;
            editId = id;
            
            if (id && S.cards[type][id]) {
                const card = S.cards[type][id];
                mH.textContent = 'Edit ' + type.slice(0, -1);
                mT.value = card.title || '';
                mD.value = card.desc || '';
                mDrive.value = card.drive || '';
                orgSelect.value = card.org || '';
                dueDateInput.value = card.dueDate || '';
                linksTemp = [...(card.links || [])];
                filesTemp = [];
                selectedPriority = card.priority || 'medium';
                
                // Update priority selector
                $$('.priority-option').forEach(opt => {
                    opt.classList.toggle('selected', opt.dataset.priority === selectedPriority);
                });
            } else {
                mH.textContent = 'New ' + type.slice(0, -1);
                mT.value = '';
                mD.value = '';
                mDrive.value = '';
                orgSelect.value = '';
                dueDateInput.value = '';
                linksTemp = [];
                filesTemp = [];
                selectedPriority = 'medium';
                
                // Reset priority selector
                $$('.priority-option').forEach(opt => {
                    opt.classList.toggle('selected', opt.dataset.priority === 'medium');
                });
            }
            
            // Show/hide due date based on type
            $('#dueDateGroup').style.display = type === 'milestones' ? 'block' : 'none';
            
            mFile.value = '';
            drawLinks();
            drawFiles();
            ov.classList.add('show');
            mT.focus();
        };

        const closeModal = () => {
            ov.classList.remove('show');
            curType = null;
            editId = null;
        };

        // Save item
        saveBtn.onclick = async () => {
            const title = mT.value.trim();
            if (!title) {
                alert('Title is required');
                return;
            }

            const id = editId || S.next++;
            
            const existingCard = editId ? S.cards[curType][id] : null;
            const existingFiles = existingCard ? existingCard.files || [] : [];

            const newFiles = filesTemp.map(f => ({
                id: 'file_' + Date.now() + '_' + Math.random().toString(36).substr(2, 9),
                name: f.name,
                size: f.size,
                type: f.type,
                uploadDate: new Date().toISOString(),
                inVault: false
            }));

            const allFiles = [...existingFiles, ...newFiles];

            S.cards[curType][id] = {
                id,
                title,
                desc: mD.value.trim(),
                drive: mDrive.value.trim(),
                links: [...linksTemp],
                files: allFiles,
                priority: selectedPriority,
                org: orgSelect.value,
                dueDate: dueDateInput.value,
                created: existingCard ? existingCard.created : new Date().toISOString(),
                updated: new Date().toISOString(),
                completed: existingCard ? existingCard.completed : false
            };

            saveToLocal();
            closeModal();
            updateStats();
            renderCards();
            
            if (newFiles.length > 0) {
                showNotification(`Added ${newFiles.length} file(s) to ${title}`);
            }
        };

        // Cancel button
        cancelBtn.onclick = closeModal;

        // Close modal when clicking overlay
        ov.onclick = e => {
            if (e.target === ov) closeModal();
        };
        
        // Close vault when clicking overlay
        $('#vaultOverlay').onclick = e => {
            if (e.target === $('#vaultOverlay')) closeVault();
        };

        // Add new item buttons
        $$('.add-btn', workspace).forEach(btn => {
            btn.onclick = () => openModal(btn.dataset.type);
        });

        // Toggle theme for a card
        window.toggleCardTheme = (cardType, cardId) => {
            const key = `${cardType}_${cardId}`;
            S.themes[key] = S.themes[key] === 'dark' ? 'light' : 'dark';
            saveToLocal();
            renderCards();
        };

        // Move file to vault
        window.moveFileToVault = (cardType, cardId, fileId) => {
            const card = S.cards[cardType][cardId];
            if (!card) return;
            
            const fileIndex = card.files.findIndex(f => f.id === fileId);
            if (fileIndex === -1) return;
            
            const file = card.files[fileIndex];
            
            // Check if file already in vault
            if (file.inVault) {
                showNotification(`"${file.name}" is already in the vault`);
                return;
            }
            
            S.vault.push({
                id: fileId,
                name: file.name,
                size: file.size,
                type: file.type,
                date: new Date().toISOString(),
                source: `${cardType}: ${card.title}`,
                driveId: null
            });
            
            card.files[fileIndex].inVault = true;
            
            saveToLocal();
            renderCards();
            updateStats();
            showNotification(`✅ "${file.name}" added to vault`);
            
            // Update vault badge immediately
            $('#vaultBadge').textContent = S.vault.length;
        };

        // Remove file from card
        window.removeFileFromCard = (cardType, cardId, fileId) => {
            const card = S.cards[cardType][cardId];
            if (!card) return;
            
            const fileIndex = card.files.findIndex(f => f.id === fileId);
            if (fileIndex === -1) return;
            
            const file = card.files[fileIndex];
            
            if (confirm(`Remove "${file.name}" from this item?`)) {
                card.files.splice(fileIndex, 1);
                saveToLocal();
                renderCards();
                updateStats();
            }
        };

        // Toggle completion status
        window.toggleComplete = (cardType, cardId) => {
            const card = S.cards[cardType][cardId];
            if (!card) return;
            
            card.completed = !card.completed;
            saveToLocal();
            updateStats();
            renderCards();
        };

        // Render cards
        const renderCards = () => {
            TYPES.forEach(type => {
                const grid = $('#grid_' + type);
                const cards = Object.values(S.cards[type] || {});
                
                grid.innerHTML = cards.length ? 
                    cards.map(card => {
                        const themeKey = `${type}_${card.id}`;
                        const isDark = S.themes[themeKey] === 'dark';
                        const themeClass = isDark ? 'dark-theme' : '';
                        const themeIcon = isDark ? '☀️' : '🌙';
                        
                        // Priority badge
                        const priorityBadge = card.priority ? 
                            `<span class="priority-badge priority-${card.priority}">${
                                card.priority === 'high' ? 'High Priority' : 
                                card.priority === 'medium' ? 'Medium' : 'Low'
                            }</span>` : '';
                        
                        // Organization tags
                        const orgTags = card.org ? 
                            `<div class="status-tags">
                                <span class="status-tag tag-${card.org}">
                                    ${card.org === 'at' ? 'AT' : card.org === 'ac' ? 'AC' : 'Integrated'}
                                </span>
                            </div>` : '';
                        
                        // Due date display for milestones
                        const dueDateDisplay = type === 'milestones' && card.dueDate ? 
                            `<div style="margin-top:8px;font-size:0.85rem;color:#6b7280">
                                📅 Due: ${new Date(card.dueDate).toLocaleDateString()}
                            </div>` : '';
                        
                        const filesHtml = card.files && card.files.length ? 
                            `<div class="file-list">
                                <strong style="font-size:.85rem">📎 Documents (${card.files.length})</strong>
                                ${card.files.map(f => `
                                    <div class="file-item">
                                        <div class="file-info">
                                            <span>${f.name}</span>
                                            ${f.inVault ? '<span class="vault-status">✓ In Vault</span>' : ''}
                                        </div>
                                        <div class="file-actions">
                                            ${!f.inVault ? 
                                                `<button class="pill purple" onclick="moveFileToVault('${type}', ${card.id}, '${f.id}')">→ Vault</button>` : 
                                                ''
                                            }
                                            <button class="pill red" onclick="removeFileFromCard('${type}', ${card.id}, '${f.id}')">Remove</button>
                                        </div>
                                    </div>
                                `).join('')}
                            </div>` : '';
                        
                        return `
                            <div class="card ${themeClass}" data-id="${card.id}">
                                ${priorityBadge}
                                <button class="theme-toggle" onclick="toggleCardTheme('${type}', ${card.id})" title="Toggle theme">${themeIcon}</button>
                                <button class="del" data-id="${card.id}" data-type="${type}">×</button>
                                <h3>${card.title}</h3>
                                ${card.desc ? `<p>${card.desc}</p>` : ''}
                                ${orgTags}
                                ${dueDateDisplay}
                                ${card.drive ? `<div class="link-list"><a href="${card.drive}" target="_blank">📁 Drive Folder</a></div>` : ''}
                                ${card.links && card.links.length ? `<div class="link-list">${card.links.map(l => `<a href="${l}" target="_blank">${l}</a>`).join('')}</div>` : ''}
                                ${filesHtml}
                                <div class="actions">
                                    <button class="pill blue edit-btn" data-id="${card.id}" data-type="${type}">✏️ Edit</button>
                                    ${type === 'workstreams' || type === 'milestones' ? 
                                        `<button class="pill ${card.completed ? 'green' : 'yellow'}" onclick="toggleComplete('${type}', ${card.id})">
                                            ${card.completed ? '✓ Complete' : '○ In Progress'}
                                        </button>` : 
                                        '<button class="pill green">Active</button>'
                                    }
                                    <button class="pill gray">Updated ${new Date(card.updated).toLocaleDateString()}</button>
                                </div>
                            </div>
                        `;
                    }).join('') :
                    '<p style="text-align:center;color:#6b7280;grid-column:1/-1;padding:40px">No items yet. Click the + button to add one!</p>';
            });

            // Add event listeners
            $$('.edit-btn', workspace).forEach(btn => {
                btn.onclick = e => {
                    e.stopPropagation();
                    openModal(btn.dataset.type, +btn.dataset.id);
                };
            });

            $$('.del', workspace).forEach(btn => {
                btn.onclick = e => {
                    e.stopPropagation();
                    if (confirm('Delete this item?')) {
                        const themeKey = `${btn.dataset.type}_${btn.dataset.id}`;
                        delete S.themes[themeKey];
                        delete S.cards[btn.dataset.type][btn.dataset.id];
                        saveToLocal();
                        updateStats();
                        renderCards();
                    }
                };
            });
        };

        // Vault functionality
        let currentVaultFilter = 'all';
        let currentVaultSort = 'date-desc';
        let vaultSearchTerm = '';

        // Open vault
        window.openVault = () => {
            $('#vaultOverlay').classList.add('show');
            
            // Show loading state briefly
            const content = $('#vaultContent');
            content.innerHTML = `
                <div class="vault-empty">
                    <div class="vault-empty-icon">⏳</div>
                    <p>Loading vault...</p>
                </div>
            `;
            
            // Render after a brief delay for better UX
            setTimeout(() => {
                renderVault();
                updateVaultStats();
                // Focus search input
                $('#vaultSearch').focus();
            }, 100);
        };

        // Close vault
        window.closeVault = () => {
            $('#vaultOverlay').classList.remove('show');
            // Filters and search are preserved for next time
        };

        // Search vault
        window.searchVault = () => {
            vaultSearchTerm = $('#vaultSearch').value.toLowerCase();
            $('#searchClear').style.display = vaultSearchTerm ? 'flex' : 'none';
            renderVault();
        };
        
        // Clear search
        window.clearSearch = () => {
            $('#vaultSearch').value = '';
            vaultSearchTerm = '';
            $('#searchClear').style.display = 'none';
            renderVault();
        };
        
        // Reset filters
        window.resetFilters = () => {
            clearSearch();
            $('#vaultFilter').value = 'all';
            $('#vaultSort').value = 'date-desc';
            currentVaultFilter = 'all';
            currentVaultSort = 'date-desc';
            renderVault();
        };

        // Filter vault
        window.filterVault = () => {
            currentVaultFilter = $('#vaultFilter').value;
            renderVault();
        };

        // Sort vault
        window.sortVault = () => {
            currentVaultSort = $('#vaultSort').value;
            renderVault();
        };

        // Get file type
        function getFileType(filename) {
            const ext = filename.split('.').pop().toLowerCase();
            if (['pdf'].includes(ext)) return 'pdf';
            if (['doc', 'docx', 'txt'].includes(ext)) return 'doc';
            if (['xls', 'xlsx', 'csv'].includes(ext)) return 'xls';
            if (['jpg', 'jpeg', 'png', 'gif', 'bmp'].includes(ext)) return 'img';
            return 'other';
        }

        // Get file icon
        function getFileIcon(type) {
            switch(type) {
                case 'pdf': return '📄';
                case 'doc': return '📝';
                case 'xls': return '📊';
                case 'img': return '🖼️';
                default: return '📎';
            }
        }

        // Format file size
        function formatFileSize(bytes) {
            if (bytes < 1024) return bytes + ' B';
            if (bytes < 1024 * 1024) return (bytes / 1024).toFixed(1) + ' KB';
            return (bytes / (1024 * 1024)).toFixed(1) + ' MB';
        }

        // Update vault stats
        function updateVaultStats() {
            const total = S.vault.length;
            const totalSize = S.vault.reduce((sum, file) => sum + (file.size || 0), 0);
            
            const oneWeekAgo = new Date();
            oneWeekAgo.setDate(oneWeekAgo.getDate() - 7);
            const recent = S.vault.filter(file => new Date(file.date) > oneWeekAgo).length;

            $('#vaultTotal').textContent = total;
            $('#vaultSize').textContent = formatFileSize(totalSize);
            $('#vaultRecent').textContent = recent;
            
            // Enable/disable clear button
            const clearBtn = $('#clearVaultBtn');
            if (clearBtn) {
                if (total === 0) {
                    clearBtn.disabled = true;
                    clearBtn.style.background = '#6b7280';
                } else {
                    clearBtn.disabled = false;
                    clearBtn.style.background = '#dc2626';
                }
            }
        }

        // Render vault
        function renderVault() {
            const content = $('#vaultContent');
            let files = [...S.vault];

            // Apply search filter
            if (vaultSearchTerm) {
                files = files.filter(file => 
                    file.name.toLowerCase().includes(vaultSearchTerm) ||
                    (file.source && file.source.toLowerCase().includes(vaultSearchTerm))
                );
            }

            // Apply type filter
            if (currentVaultFilter !== 'all') {
                files = files.filter(file => getFileType(file.name) === currentVaultFilter);
            }

            // Apply sort
            files.sort((a, b) => {
                switch(currentVaultSort) {
                    case 'date-desc':
                        return new Date(b.date) - new Date(a.date);
                    case 'date-asc':
                        return new Date(a.date) - new Date(b.date);
                    case 'name-asc':
                        return a.name.localeCompare(b.name);
                    case 'name-desc':
                        return b.name.localeCompare(a.name);
                    case 'size-desc':
                        return (b.size || 0) - (a.size || 0);
                    case 'size-asc':
                        return (a.size || 0) - (b.size || 0);
                    default:
                        return 0;
                }
            });

            // Show filter status if active
            const hasActiveFilters = vaultSearchTerm || currentVaultFilter !== 'all';
            const filterStatus = hasActiveFilters ? 
                `<div style="display:flex;align-items:center;justify-content:center;gap:12px;padding:10px;background:#f3f4f6;border-radius:8px;margin-bottom:16px;font-size:0.9rem;color:#6b7280">
                    <span>
                        Showing ${files.length} of ${S.vault.length} files
                        ${vaultSearchTerm ? ` matching "${vaultSearchTerm}"` : ''}
                        ${currentVaultFilter !== 'all' ? ` (${currentVaultFilter} files only)` : ''}
                    </span>
                    <button class="pill gray" onclick="resetFilters()">Reset Filters</button>
                </div>` : '';

            if (files.length === 0) {
                content.innerHTML = filterStatus + `
                    <div class="vault-empty">
                        <div class="vault-empty-icon">📂</div>
                        <p>${hasActiveFilters ? 'No files match your search criteria' : 'No documents in vault yet'}</p>
                        <p style="font-size:0.9rem;margin-top:8px;color:#6b7280">
                            ${hasActiveFilters ? 'Try adjusting your filters or search term' : 'Move documents to vault from any workspace item'}
                        </p>
                    </div>
                `;
            } else {
                content.innerHTML = filterStatus + files.map(file => {
                    const fileType = getFileType(file.name);
                    const icon = getFileIcon(fileType);
                    
                    return `
                        <div class="vault-item">
                            <div class="vault-item-icon ${fileType}">${icon}</div>
                            <div class="vault-item-info">
                                <div class="vault-item-name">${file.name}</div>
                                <div class="vault-item-meta">
                                    <span>📅 ${new Date(file.date).toLocaleDateString()}</span>
                                    <span>💾 ${formatFileSize(file.size || 0)}</span>
                                    <span>📍 ${file.source || 'Unknown source'}</span>
                                </div>
                            </div>
                            <div class="vault-item-actions">
                                <button class="pill blue" onclick="downloadFile('${file.id}')">📥 Download</button>
                                <button class="pill red" onclick="removeFromVault('${file.id}')">Remove</button>
                            </div>
                        </div>
                    `;
                }).join('');
            }
        }

        // Download file (simulated)
        window.downloadFile = (fileId) => {
            const file = S.vault.find(f => f.id === fileId);
            if (file) {
                showNotification(`Downloading ${file.name}...`);
                // In a real app, this would trigger actual file download
            }
        };

        // Remove from vault
        window.removeFromVault = (fileId) => {
            const file = S.vault.find(f => f.id === fileId);
            if (file && confirm(`Remove "${file.name}" from vault?`)) {
                S.vault = S.vault.filter(f => f.id !== fileId);
                
                // Update the original card's file status
                TYPES.forEach(type => {
                    Object.values(S.cards[type] || {}).forEach(card => {
                        if (card.files) {
                            card.files.forEach(f => {
                                if (f.id === fileId) {
                                    f.inVault = false;
                                }
                            });
                        }
                    });
                });
                
                saveToLocal();
                renderVault();
                updateVaultStats();
                renderCards();
                showNotification(`Removed "${file.name}" from vault`);
                
                // Update vault badge
                $('#vaultBadge').textContent = S.vault.length;
            }
        };

        // Export vault list
        window.exportVaultList = () => {
            const vaultData = S.vault.map(file => ({
                name: file.name,
                date: new Date(file.date).toLocaleDateString(),
                size: formatFileSize(file.size || 0),
                source: file.source || 'Unknown',
                type: getFileType(file.name)
            }));
            
            const csvContent = 'data:text/csv;charset=utf-8,' + 
                'Name,Date Added,Size,Source,Type\n' +
                vaultData.map(f => `"${f.name}","${f.date}","${f.size}","${f.source}","${f.type}"`).join('\n');
            
            const link = document.createElement('a');
            link.setAttribute('href', encodeURI(csvContent));
            link.setAttribute('download', `vault_export_${new Date().toISOString().split('T')[0]}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showNotification('Vault list exported as CSV');
        };

        // Clear vault
        window.clearVault = () => {
            if (S.vault.length === 0) {
                showNotification('Vault is already empty');
                return;
            }
            
            if (confirm(`Are you sure you want to remove all ${S.vault.length} files from the vault? This action cannot be undone.`)) {
                // Update all cards to reflect files no longer in vault
                S.vault.forEach(vaultFile => {
                    TYPES.forEach(type => {
                        Object.values(S.cards[type] || {}).forEach(card => {
                            if (card.files) {
                                card.files.forEach(f => {
                                    if (f.id === vaultFile.id) {
                                        f.inVault = false;
                                    }
                                });
                            }
                        });
                    });
                });
                
                S.vault = [];
                saveToLocal();
                renderVault();
                updateVaultStats();
                renderCards();
                showNotification('Vault cleared successfully');
                
                // Update vault badge
                $('#vaultBadge').textContent = '0';
            }
        };

        // Initialize
        loadLocalData();
        updateStats();
        renderCards();

        // Set initial view
        ($('.tab[data-type="' + S.view + '"]', workspace) || $('.tab', workspace)).click();
        
        // Set vault button tooltip based on OS
        const isMac = navigator.platform.toUpperCase().indexOf('MAC') >= 0;
        $('#openVaultTop').title = `Open Document Vault (${isMac ? 'Cmd' : 'Ctrl'}+K)`;
        
        // Keyboard shortcuts
        document.addEventListener('keydown', (e) => {
            // ESC to close modals
            if (e.key === 'Escape') {
                if ($('#modalOv').classList.contains('show')) {
                    closeModal();
                } else if ($('#vaultOverlay').classList.contains('show')) {
                    closeVault();
                }
            }
            
            // Ctrl+K or Cmd+K to open vault
            if ((e.ctrlKey || e.metaKey) && e.key === 'k') {
                e.preventDefault();
                if (!$('#vaultOverlay').classList.contains('show')) {
                    openVault();
                }
            }
        });
    })();
    </script>
</body>
</html>
