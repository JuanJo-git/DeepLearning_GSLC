| exp | arch | strategy | augmentation | trainable_params | lr_groups | best_epoch | epochs_run | train_time_s | train_acc_clean | val_acc | test_macro_f1 | gap_train_val |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| B1_scratch | resnet18 | B1 | True | 11181642 | all=0.001 | 23 | 28 | 213.6000 | 0.7251 | 0.6522 | 0.6548 | 0.0730 |
| B2_feature_extraction | resnet18 | B2 | True | 5130 | head=0.001 | 15 | 20 | 147.7500 | 0.9928 | 0.9799 | 0.9668 | 0.0129 |
| B3_progressive_ft | resnet18 | B3 | True | 8398858 | last_blocks=0.0001; head=0.001 | 6 | 11 | 80.1900 | 0.9964 | 0.9900 | 0.9699 | 0.0065 |
| B4_B3_noaug | resnet18 | B3 | False | 8398858 | last_blocks=0.0001; head=0.001 | 4 | 9 | 45.3500 | 1.0000 | 0.9766 | 0.9732 | 0.0234 |
| B4_B3_aug | resnet18 | B3 | True | 8398858 | last_blocks=0.0001; head=0.001 | 6 | 11 | 80.5300 | 0.9964 | 0.9900 | 0.9699 | 0.0065 |
| B5_efficientnet_b0 | efficientnet_b0 | B3 | True | 1142202 | last_blocks=0.0001; head=0.001 | 8 | 13 | 95.0400 | 0.9993 | 0.9933 | 0.9801 | 0.0060 |
| B5_mobilenet_v3_small | mobilenet_v3_small | B3 | True | 1245738 | last_blocks=0.0001; head=0.001 | 5 | 10 | 71.9000 | 0.9936 | 0.9331 | 0.9669 | 0.0604 |