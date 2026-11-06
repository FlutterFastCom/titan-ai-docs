---
id: explain/codes-run-text-media
audience: both
intents: [text generation failed, caption not written, photo picker failed, music panel, video converter, post not found for run, post binding missing, encoder unavailable]
tools: [list_adapters, probe_user_proxy, get_host_deps, device_detail, list_resources, read_run_transcript, get_user_message_screenshot, wait_seconds]
---
# Text writing, photo picker, music and post codes inside a run

These come from Titan's run AI while it wrote text, picked photos or sound, or published a post. You see them in a run's
steps and transcript (owned strategies only). Fix the cause, then run again; never repeat a posting step before checking
whether it already posted.

## Text writing
| code | meaning | can you fix it | ask the user | only Titan |
|---|---|---|---|---|
| `text_generate_binding_missing` | the step's saved prompt is not properly attached to this run | own strategy: check the step and the run settings pick the saved prompt (`build/7-improve-it.md`), run again | shared strategy: "This needs a fix by the person who made the strategy." | — |
| `text_generate_run_required` | text writing works only inside a real run and was called outside one | no | — | yes if it appears in a normal run |
| `text_generate_prompt_not_found` | the saved prompt the step uses no longer exists | own strategy: pick an existing saved prompt in the step | "The text template this strategy uses was deleted. Which one should it use instead?" | — |
| `text_generate_unavailable` | writing failed: the AI service did not answer or timed out, or its setup (service, proxy, length limit) could not be used | check the AI service (`list_adapters`) and the proxy (`probe_user_proxy`), then run again | if the service is broken: "Your AI service isn't answering. Please check its key or balance in Titan." | if the service is fine |
| `text_generate_invalid_output` | the AI service answered with no usable plain text | run again once; if repeated, try another AI service | — | — |
| `text_generate_over_length` | the text was longer than the prompt's limit, twice in a row | own strategy: shorten the instructions or raise the length limit in the saved prompt | — | — |

## Photo picker, music, video
| code | meaning | can you fix it | ask the user | only Titan |
|---|---|---|---|---|
| `picker_capture_failed` | Titan could not take a picture of the phone's photo picker | check the phone (`device_detail`), run again | "Keep the phone unlocked and awake while it picks photos." | — |
| `picker_decode_failed` | the picture of the picker came back unreadable | run again | — | if it repeats |
| `picker_state_unavailable` | Titan could not compare two pictures of the picker to tell whether it stopped moving | run again | — | if it repeats |
| `picker_map_missing` | the run tried to tap photo numbers before the picker was scanned (a step-order problem) | no | shared strategy: "This needs a fix by the person who made the strategy." | — |
| `picker_ids_missing` | none of the photo numbers it tried to tap exist in the last scan | run again | — | — |
| `music_panel_unavailable` | Titan could not take a picture of the music panel, so it could not choose a sound | check the phone, run again | "Keep the phone unlocked and awake while it posts." | — |
| `encoder_unavailable` | the PC Agent has not reported a working video converter for its current session, so videos cannot be prepared | `get_host_deps` to see what the PC lacks | "Please restart the Titan PC Agent so it can prepare videos, and tell me if it still fails." | if it still fails |

## The post
| code | meaning | can you fix it | ask the user | only Titan |
|---|---|---|---|---|
| `publish_post_binding_missing` | the run has no post chosen for the posting step | start the run again with a post selected | "Which post should this account publish?" | — |
| `publish_post_not_found` | the post chosen at start was deleted or changed after the run started | pick a current post and run again | "That post was changed or deleted. Use the new version?" | — |
