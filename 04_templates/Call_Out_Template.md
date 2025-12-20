 <%*
list = {
"I: Info" : "info",
"N: Note" : "note",
"S: Summary" : "summary",
"T: Tip" : "tip",
"C: Check" : "check",
"H: Help" : "help",
"W: Warning" : "warning",
"F: Fail" : "fail",
"D: Danger" : "danger",
"B: Bug" : "bug",
"E: Example" : "example",
"Q: Quote " : "quote"
};
k = Object.keys(list);

x = await tp.system.suggester(k, k);
if (x) return ">[!" + list[x] + "]+ " + list[x] + "\n> ";
%>