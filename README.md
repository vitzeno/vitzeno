
Software engineer in London. Most of what I write is in Go.

### [detent](https://github.com/vitzeno/detent)

An agent harness for the terminal. Commands run on your machine, and anything flagged dangerous waits for you to approve it. You can undo a request, review what it changed and hand work off to subagents.

<img src="https://raw.githubusercontent.com/vitzeno/detent/main/.github/assets/review_2.png" alt="Reviewing a request in detent: changed files on the left, the diff on the right with a reviewer's comment under a line">

### [sift](https://github.com/vitzeno/sift)

A small, typed language for writing ETL pipelines. Mark a field `@pii` and sift tracks it through every step, renames included. If it reaches a sink without being masked, the program won't compile.

```sift
source customers = csv("customers.csv", schema: {
  id: int,
  name: string,
  email: string @pii,
  phone: string @pii,
  plan: string
})
sink warehouse = jsonl("warehouse.jsonl")

pipeline export {
  customers
    |> filter(.plan != "trial")
    |> rename(phone: contact)
    |> hash(email)
    |> select(id, name, email, contact, plan)
    |> warehouse
}
```

```console
$ sift run export.sift
export.sift:10:1: error: field "contact" is @pii and reaches sink "warehouse" unmasked; declassify with mask/hash/redact
```

### Other projects

- [llvm](https://github.com/vitzeno/llvm): a C-like language built on LLVM
- [lsp](https://github.com/vitzeno/lsp): a basic language server
- [postgres](https://github.com/vitzeno/postgres): a Raycast extension for exploring Postgres databases
