# Align three analyzer filters for shared Guides

Status: plan only, awaiting review. No documentation content has changed.

## Scope and immutable inputs

Align Lowercase, ASCII folding, and Alphanumonly on `v3.0.x`, then apply the reviewed changes on a separate branch based on `preview`. Exclude Whitespace, StructArray, SDK Reference, zdoc onboarding, and activation.

| Input | Revision |
| --- | --- |
| Milvus v3.0.x | [79341af3be75a66425e5972ea26d1d9c90705fd7](https://github.com/milvus-io/milvus-docs/commit/79341af3be75a66425e5972ea26d1d9c90705fd7) |
| Milvus preview | [2232dea4ebfb7c22864eca00ffc6a53264f3bf03](https://github.com/milvus-io/milvus-docs/commit/2232dea4ebfb7c22864eca00ffc6a53264f3bf03) |
| Cloud generated Guides/BYOC | [387e287244ccd586052bb3d7c22669f634e17cad](https://github.com/zilliztech/zdoc/commit/387e287244ccd586052bb3d7c22669f634e17cad) |

The three Milvus source blobs match between these version branches. Recheck refs and page blobs before implementation and before preparing the preview branch. Preserve changes made after these inputs rather than overwriting them.

Allowed implementation paths:

- `site/en/userGuide/schema/analyzer/filter/lowercase-filter.md`
- `site/en/userGuide/schema/analyzer/filter/ascii-folding-filter.md`
- `site/en/userGuide/schema/analyzer/filter/alphanumonly-filter.md`

This plan is the only allowed extra tracked file during review. Do not change global variables, navigation, dependencies, scripts, workflows, other pages, or generated output. Remove this plan with a normal commit before marking the content change ready.

## Per-page code inventory

Each page retains three code groups and a separate expected-output block. Each group must contain exactly one Python, Java, Go, Rust, C++, NodeJS, C#, and cURL example after alignment. Use `cpp` for the Milvus fence and scaffold, following the aligned Standard Tokenizer.

| Group | Existing Milvus examples | Alignment for each page |
| --- | --- | --- |
| Configuration | Python, Java, NodeJS, Go, cURL | Preserve intent; add Rust, C++, C# from reviewed Cloud examples |
| Analyzer configuration | Python, Java, Go; NodeJS/cURL comment placeholders | Add Rust, C++, C#; replace both placeholders with actual configuration |
| Verification | Python, Java, Go; NodeJS/cURL comment placeholders | Add Rust, C++, C#; replace both placeholders with actual analyzer calls |

Reconcile **9 added blocks and 4 filled placeholders per page**, or **27 additions and 12 placeholder replacements** in total. Count actual nonempty examples, not just fences. Preserve the separate expected-output block and each page's analyzer configuration, sample text, accented characters, and expected tokens.

Cloud input paths are `content/en/guides/tutorials/development/analyzer/analyzer-filters/<slug>.md` and the corresponding BYOC paths at the pinned Cloud commit. Their bodies match after front matter is removed. They are comparison inputs, not proof that the SDK examples are correct.

## Shared source adaptations

1. Use existing `{{var.product_name}}`, `{{var.example_endpoint}}`, and `{{var.example_token}}` references; do not change their definitions. Match the aligned [Standard Tokenizer](https://github.com/milvus-io/milvus-docs/blob/79341af3be75a66425e5972ea26d1d9c90705fd7/site/en/userGuide/schema/analyzer/tokenizer/standard-tokenizer.md) conventions.
2. Preserve SDK identifiers such as `MilvusClient`, package names, and imports. The Alphanumonly input `Milvus 2.0 @ Scale! #AI #Vector_Databasé` is test data: do not replace it with a product variable. A precise Cloud product-name allowance belongs to the later zdoc onboarding change.
3. Remove the `| Milvus 2.5.11+` suffix from the verification headings on these two 3.x branches, matching Standard Tokenizer. Retain the complete verification sections. No changes to 2.5/2.6 branches are included.
4. Preserve front matter id, title, summary intent, headings, prose, and navigation unless a listed adaptation requires a change. Product references in prose may use the product variable; do not perform an unrelated rewrite.
5. Use Milvus `multipleCode` scaffolds and ordinary fenced blocks; do not copy Cloud Tabs, TabItem, Admonition, or imports. Keep scaffolds and fences consistent.
6. Do not introduce named blocks without an actual Cloud-only transform. This batch should use shared examples and variables; if a semantic product difference emerges, document it before adding a transform target.

## Known defects in the Cloud comparison inputs

| Page | Defect | Planned correction and evidence requirement |
| --- | --- | --- |
| Lowercase, ASCII folding | C# uses `Username = "root"` and `Password = "Milvus"` | Confirm `ConnectConfig.ApiKey` in the target SDK source, then use the existing token variable; do not propagate user/password literals |
| Lowercase, ASCII folding | NodeJS constructs `MilvusClient` with an address but no token | Confirm the SDK config field and include the existing token variable |
| ASCII folding | Rust verification calls `client.run_analyzer` without defining a client in any preceding example | Add the verified V2 client initialization with endpoint/token, following the aligned page and SDK source |

Check the other added examples for imports, request fields, response accessors, and usable client construction. List any additional SDK correction separately; do not silently alter sample behavior to make a checker pass. Preserve valid original Python/Java/Go behavior while adding shared connection authentication. Source verification does not substitute for executing an example.

## SDK source evidence

The following published tags were resolved to immutable source commits. This is API-shape verification, not compiled or live execution evidence.

| SDK | Source evidence | Verified use |
| --- | --- | --- |
| C# v3.0.0 | [ConnectConfig](https://github.com/milvus-io/milvus-sdk-csharp/blob/32d73b399c9542d3063b6c489a535d6cfd2e60e6/Milvus.Client.V2/Types/ConnectConfig.cs#L28), [client constructor](https://github.com/milvus-io/milvus-sdk-csharp/blob/32d73b399c9542d3063b6c489a535d6cfd2e60e6/Milvus.Client.V2/MilvusClientV2.cs#L44), [RunAnalyzerAsync](https://github.com/milvus-io/milvus-sdk-csharp/blob/32d73b399c9542d3063b6c489a535d6cfd2e60e6/Milvus.Client.V2/MilvusClientV2.Utility.cs#L263) | ApiKey is supported and is mutually exclusive with Username; request/response fields match the proposed usage |
| Rust v3.0.2 | [official analyzer example](https://github.com/milvus-io/milvus-sdk-rust/blob/5a873adcad6e165b22126548df5468a2c1ac1dac/examples/v2/run_analyzer.rs), [ClientV2](https://github.com/milvus-io/milvus-sdk-rust/blob/5a873adcad6e165b22126548df5468a2c1ac1dac/src/v2/client.rs#L307) | ConnectConfig uri/token, ClientV2 initialization, builder and response traversal exist |
| C++ v3.0.4 | [official analyzer example](https://github.com/milvus-io/milvus-sdk-cpp/blob/c346ffb752414fe4719afcd7efb8864d1a5dec77/examples/src/v2/run_analyzer.cpp#L44) | ConnectParam(uri, token), AddText, WithAnalyzerParams and RunAnalyzer are demonstrated |
| NodeJS v3.0.6 | [ClientConfig and analyzer types](https://github.com/milvus-io/milvus-sdk-node/blob/d45bb1d1ef546d78c444c053f85a7820d33983ee/milvus/types/Client.ts#L10), [implementation](https://github.com/milvus-io/milvus-sdk-node/blob/d45bb1d1ef546d78c444c053f85a7820d33983ee/milvus/grpc/GrpcClient.ts#L739) | address/token, string-or-array text, analyzer_params object, and results/tokens/token are supported |
| Milvus REST v3.0.3 | [route](https://github.com/milvus-io/milvus/blob/328df0cd2f323c2d3c50cef91bfc9f2c412bcb09/internal/distributed/proxy/httpserver/handler_v2.go#L429), [request](https://github.com/milvus-io/milvus/blob/328df0cd2f323c2d3c50cef91bfc9f2c412bcb09/internal/distributed/proxy/httpserver/request_v2.go#L1317) | POST /v2/vectordb/common/run_analyzer; analyzerParams must be a JSON-encoded string and text a string array |

Remaining execution checks: imports and compilation context for added examples, C# collection-expression compiler compatibility, REST Bearer middleware, and actual analyzer outputs. Preserve valid Python/Java/Go examples; their API surface was not part of this focused source audit. No private credentials or production calls are needed for plan approval.

## Links and expected changes

Preserve `analyzer-overview.md#Example-use`; confirm the referenced Milvus heading still exists and record the normalized Cloud `#example-use` target for the later generated-output check. Preserve ASCII folding's Wikipedia reference and Standard Tokenizer link.

Expected differences from the old Milvus source: 27 new language blocks, 12 completed placeholders, product/connection variables, required authentication/client setup, and removal of the obsolete heading suffix. Expected differences from Cloud: Milvus markup and variables, plus the explicit SDK corrections above. Every other code/prose change requires a line-level explanation in the implementation report.

## Verification plan

Run these in the exact implementation checkout and repeat relevant checks on the preview candidate:

- `node check-link.js` — repository-wide page-id, navigation, and internal-link checks.
- `python3 .skills/edit-docs/scripts/verify.py --diff 79341af3be75a66425e5972ea26d1d9c90705fd7` — use the preview base SHA for that branch. A plan-only run checking zero pages is not content validation.
- `npx --yes --package @zilliz/mdtomd@1.3.0 md2md-blocks check site/en` — the package version used by the branch's marker CI.
- `git diff --check` and an exact changed-path allowlist check.
- Per-page inventory: three groups of eight unique languages, no comment-only placeholder, unchanged expected output and sample strings. Verify each of the 27 additions and 12 replacements individually.
- Resolve the pinned md2md 1.3.0 preprocessing API from its shipped documentation/source and use it locally for the three pages. Record the actual command, ensure variables expand to Milvus values, and retain outputs outside tracked source. Do not invent a CLI flag or run upload/deployment scripts.
- Compare the candidate's three files on v3.0.x and preview. If the bases remain identical, resulting page blobs must match; otherwise explain and review branch-specific differences.

The root `npm test` script is an intentional failing placeholder and is not a validation gate. Do not upload images, invoke production endpoints with real credentials, trigger Fetch/Translation, or write Base. SDK execution or a full site build that was not performed must remain explicitly unverified.

### Baseline evidence already collected

On the unchanged v3.0.x source, `npm ci --ignore-scripts --no-audit --no-fund` completed and `node check-link.js` exited 0. The worktree was clean afterward. No modified-page verifier, marker check, preprocessing, SDK execution, or site build has yet been claimed.

## Review and delivery sequence

1. Commit this plan alone as `wip: ... plan` with DCO sign-off. Review the exact plan diff before implementation.
2. The fork's v3.0.x is behind upstream. Prepare a new fixed review-base branch at the pinned upstream commit so the fork draft PR shows only this plan. Do not reset or force-update the fork's version branch.
3. Show the driver the English PR draft and exact push/create commands. Push the two new branches and open the fork draft PR only after authorization, then stop for a ruling.
4. After the plan ruling, implement only the three allowed files and validate them. Remove this plan with a normal commit before ready. Preserve review history; do not rebase, squash, force-push, or reset --hard. This task's explicit plan/history process takes precedence over the general one-commit skill workflow.
5. Prepare separate upstream v3.0.x and preview PR descriptions and commands. The driver creates upstream PRs. Never merge them automatically. zdoc onboarding begins only after the alignment is merged.

No external comments, maintainer messages, production writes, or activation are authorized by this plan.
