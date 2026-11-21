# Data dictionary

## Identifier and fold files

`data_derived/record_id_index.csv` contains one row per image record. `record_id` is a stable de-identified hash used for joins. `source` is `YouTube` or `NewSelfie`. `label_index` follows `0 = Left`, `1 = Center`, `2 = Right`. `split_group_id` is the confirmed collection or duplicate group used for group-aware validation. `source_internal_fold` is the locked five-fold outer fold used by the source-internal OOF experiments. No event-level or player-level independence is claimed.

`data_derived/folds_groups.csv` is the fold-only view used by the training scripts. It does not contain raw image paths.

## Pose features

`data_derived/keypoints_17coco.csv` contains 17 COCO joints. Each joint has `x_norm`, `y_norm` and `confidence`. Coordinates are normalized by the original image width and height. Values are zeroed when the original confidence is below 0.3, matching Pose B1. The file also retains source, label, group and fold fields needed for source-only training and final scoring.

## Visual features

`data_derived/embeddings_resnet18_512d.csv` contains one 512-dimensional `float32` ResNet18 ImageNet V1 embedding per `record_id`. Rows follow `embedding_row` in the locked visual embedding index. The backbone was frozen. The source-only classifier fits `StandardScaler` and class-balanced LogisticRegression on the source partition only.

## Prediction files

`data_derived/predictions/` contains de-identified OOF and cross-source probability tables from the frozen rounds. Probability columns are `p_left`, `p_center`, `p_right` and sum to one within tolerance. `true_label` is retained for final evaluation and diagnostic statistics. File names retain their originating round and model role.

`data_derived/statistics/` contains the frozen 10,000-draw paired group-bootstrap summary and distribution used for the manuscript's fusion comparison.

## Source and evaluation fields

`evaluation_scope` distinguishes `source_internal_oof` from `cross_source`. `direction` uses `YouTube -> NewSelfie` or `NewSelfie -> YouTube`. `source_internal_fold` is present where the originating result saved it. No raw archive path or image filename is included in the derived feature tables.

## Workbook

`data_derived/master_evidence_workbook.xlsx` is the consolidated evidence workbook for rounds 1-33. It contains the source trace, result tables and protocol audit material used to assemble the manuscript.
