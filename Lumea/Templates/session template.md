<%*
/* ---------- CONFIG ---------- */
// Extract current session number from file name (expects: "Session X")
const path = tp.file.folder(true)
const all_file_paths = tp.app.vault.getAllLoadedFiles()
  .filter(f => typeof f.path === "string")
  .filter(f => !f.path.endsWith("Untitled.md"))
  .filter(f => f.path.startsWith(path))
  .filter(f => !f.children)  // exclude sub-folders: only files
  .map(f => f.path);

let file_nums = [];

for (const file_path of all_file_paths) {
	const split_path = file_path.split('/');
	const split_name = split_path[split_path.length - 1].split(' ');
	const num = split_name[split_name.length - 1].split('.')[0];
	file_nums.push(num);
}

file_nums.sort();

const prev_session = parseInt(file_nums[file_nums.length - 1], 10);
const curr_session = prev_session + 1;
const next_session = prev_session + 2;

const newName = `Session ${curr_session}`;
await tp.file.rename(newName);

tR += `---\n
tags:\n
  - IronDiadem\n
publish: "false"\n
---\n`

tR += `[[Session ${prev_session}|← Previous Session]]`;
tR += ` | `;
tR += `[[Session ${next_session}|Next Session →]]\n`;

tR += `# Summary\n`
%>