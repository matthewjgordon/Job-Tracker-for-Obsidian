<%*
/**
 * Listing → App (hot-reloadable)
 * When a new "... (Listing)" is created, make/fill "... (App)" and open it.
 */

// ---------- utils ----------
function onReady(fn) {
  const poll = () => {
    const app = window.app;
    if (app?.workspace) {
      if (app.workspace.layoutReady) fn(app);
      else app.workspace.onLayoutReady(() => fn(window.app));
    } else requestAnimationFrame(poll);
  };
  poll();
}

function parseListing(basename) {
  // Expect "Company - Role (Listing)"
  if (!/\(Listing\)$/.test(basename)) return null;
  const cut = basename.lastIndexOf(" - ");
  if (cut === -1) return null;
  const company = basename.slice(0, cut).trim();
  const role = basename.slice(cut + 3).replace(/\s*\((Listing|App)\)\s*$/, "").trim();
  if (!company || !role) return null;
  return { company, role };
}

function q(s) { return JSON.stringify(s); } // safe YAML double-quoted scalar

function buildYaml(company, role, today) {
  const listingTitle = `${company} - ${role} (Listing)`;
  const resumeTitle  = `Resume - User Name - ${company} ${role}.pdf`;
  const coverTitle   = `Cover Letter - User Name - ${company} ${role}.pdf`;

  return `---
type: App
status: Open
created: ${today}
company: ${company}
role: ${role}
listing: ${q(`[[${listingTitle}]]`)}
resume: "${`[[${resumeTitle}]]`}"
cover_letter: "${`[[${coverTitle}]]`}"
recruiter_call:
first_interview:
second_interview:
offer_date:
close_date:
---
`;
}

async function ensureFolder(app, destPath) {
  const adapter = app.vault.adapter;
  const folder = destPath.replace(/\\/g, "/").split("/").slice(0, -1).join("/");
  if (folder && !(await adapter.exists(folder))) await adapter.mkdir(folder);
}

// ---------- hot-reload handler ----------
onReady((app) => {
  // If an old handler exists, unregister it so edits take effect immediately.
  if (window.__listingToAppHandler) {
    app.vault.off('create', window.__listingToAppHandler);
  }

  const handler = async (file) => {
    try {
      if (!file || file.extension !== 'md') return;

      const basename = file.basename || file.name.replace(/\.md$/, '');
      const parsed = parseListing(basename);
      if (!parsed) return;

      const { company, role } = parsed;
      const today    = tp.date.now("YYYY-MM-DD");
      const appTitle = `${company} - ${role} (App)`;
      const appDir   = "Job Applications";
      const appPath  = `${appDir}/${appTitle}.md`;

      await ensureFolder(app, appPath);

      let appFile = app.vault.getAbstractFileByPath(appPath);
      const yaml  = buildYaml(company, role, today);

      if (!appFile) {
        appFile = await app.vault.create(appPath, yaml + "\n");
      } else {
        const current = await app.vault.read(appFile);
        if (!current || !current.trim()) {
          await app.vault.modify(appFile, yaml + "\n");
        } else if (!/^---\s*[\s\S]*?---\s*/.test(current)) {
          await app.vault.modify(appFile, yaml + "\n" + current);
        }
      }

      // Open the App note
      if (appFile) {
        const leaf = app.workspace.getLeaf(true);
        await leaf.openFile(appFile);
      }
    } catch (e) {
      console.error("[Listing→App] error:", e);
    }
  };

  app.vault.on('create', handler);
  window.__listingToAppHandler = handler; // save for next hot reload
});

// Startup templates must not output anything
tR = "";
%>