<%*
async function jirafy(tp) {
    let ticket = "";
    const selection = tp.file.selection();
    if (selection) {
        ticket = selection;
    } else {
        ticket = await tp.system.prompt("Jira Ticket ID", "");
    }
    
    const text = `[${ticket}](https://hybridtheory.atlassian.net/browse/${ticket})`;
    return text;
}
%><% await jirafy(tp) %>