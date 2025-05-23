# VideoComp

VideoComp provides a benchmark for training and evaluating fine-grained
video-text compositionality. It tests a model's ability to capture compositional
and temporal coherence in multi-event videos. It introduces subtle disruptions
to standard video-text pairs, such as **temporal reordering**, **action word
replacement**, and **segment-level mismatch**, enabling evaluation of models’
alignment capabilities.

This release includes two benchmark datasets:

- ActivityNet-Comp
- YouCook2-Comp

These datasets extend the dense video captioning datasets
[ActivityNet-Captions](https://cs.stanford.edu/people/ranjaykrishna/densevid/)
and [YouCook2](http://youcook2.eecs.umich.edu/), by generating LLM-rewritten
multi-event paragraphs for both positive (coherent) and negative (disrupted)
video-text pairs.

For more details, see our [CVPR 2025 paper](https://arxiv.org/abs/2504.03970).

## Dataset Format

We release `.json` annotation files for both training and validation splits.
Each file contains a list of entries with the following fields.

NOTE: For the "segment-level mismatch" task, the input video should be trimmed
by sampling the interval `query_video/{start,end}_time` from
`original_video/{start,end}_time`.

- `key`: Unique identifier
- `video_id`: YouTube video ID
- `type`: Disruptuion type
- `original_video/start_time`, `original_video/end_time`: Start/end time of the
original video
- `query_video/start_time`, `query_video/end_time`: Start/end time of the query
video (for actual train/eval)
- `positive_text`: LLM-rewritten paragraph with chronologically ordered events
- `negative_text`: LLM-rewritten paragraph with a targeted disruption
- `positive_text/start_time`, `positive_text/end_time`: Start/end time for positive text
- `negative_text/start_time`, `negative_text/end_time`: Start/end time for negative text
- `question`: Question (for LLM evaluation)
- `answer`: Answer (for LLM evaluation)

## Downloads

- [ActivityNet-Comp Train](https://storage.googleapis.com/video_comp/activitynet_comp_train.json)
- [ActivityNet-Comp Val](https://storage.googleapis.com/video_comp/activitynet_comp_val.json)
- [YouCook2-Comp Train](https://storage.googleapis.com/video_comp/youcook2_comp_train.json)
- [YouCook2-Comp Val](https://storage.googleapis.com/video_comp/youcook2_comp_val.json)

## Evaluation and Metrics

We report **binary classification accuracy**. For CLIP-style models, the task
involves comparing the similarity between the video and each of the positive and
negative texts, and predicting which one is a better match. For generative
models, we format the task as a binary-choice question as in `question`, and
check whether the model's output matches the correct answer in the form
`f"{answer}" == result` or `f"({answer})" == result`. The "all" metric is
computed as the product of the individual binary accuracies for "temporal
reordering", "action word replacement", and "segment-level mismatch".

## Citing this work

If you use this dataset or benchmark in your work, please cite:

```
@article{videocomp25,
  title={VideoComp: Advancing Fine-Grained Compositional and Temporal Alignment in Video-Text Models},
  author={Dahun Kim and AJ Piergiovanni and Ganesh Mallya and Anelia Angelova},
  booktitle={CVPR},
  year={2025}
}
```

## License and disclaimer

Copyright 2025 Google LLC

All software is licensed under the Apache License, Version 2.0 (Apache 2.0);
you may not use this file except in compliance with the Apache 2.0 license.
You may obtain a copy of the Apache 2.0 license at:
https://www.apache.org/licenses/LICENSE-2.0

All other materials are licensed under the Creative Commons Attribution 4.0
International License (CC-BY). You may obtain a copy of the CC-BY license at:
https://creativecommons.org/licenses/by/4.0/legalcode

Unless required by applicable law or agreed to in writing, all software and
materials distributed here under the Apache 2.0 or CC-BY licenses are
distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
either express or implied. See the licenses for the specific language governing
permissions and limitations under those licenses.

This is not an official Google product.
