# Project 2: DHS Data Analysis

## Logistic Regression Model
The following code described the logistic regression model.

``` R
lr_res <-
  lr_workflow %>%
  tune_grid(val_set,
            grid = lr_reg_grid,
            control = control_grid(save_pred = TRUE),
            metrics = metric_set(roc_auc))
       
lr_plot <-
  lr_res %>%
  collect_metrics() %>%
  ggplot(aes(x = penalty, y = mean)) +
  geom_point() +
  geom_line() +
  ylab("Area under the ROC Curve") +
  scale_x_log10(labels = scales::label_number())

top_models <-
  lr_res %>%
  show_best("roc_auc", n = 15) %>%
  arrange(penalty)

lr_best <-
  lr_res %>%
  collect_metrics() %>%
  arrange(penalty) %>%
  slice(10)

lr_auc <-
  lr_res %>%
  collect_predictions(parameters = lr_best) %>%
  roc_curve(wealth, .pred_1:.pred_2:.pred_3:.pred_4:.pred_5) %>%
  mutate(model = "Logistic Regression")

```

By the following plot, we noticed that the AUC stayed about the same level until it decreased drastically after the 15th observation. Considering the penalty, we decided that the 10th model gave the best prediction, yielding an average AUC of 0.664.

![](./Project2/lr_plot.png)


The Five ROC plots were shown as below:

![](./Project2/lr_auc.png)

From this data, we can tell that our model performed beautifully for wealth class 1 and 5, and moderately well for class 4. However, it had some issues distinguishing class 2 and 3. Therefore, a logical follow-up would be to merge class 2 and 3 into one single class and run the logistic regression once again.


## Random Forrest

The following code described the random forrest model.

``` R
rf_mod <-
  rand_forest(mtry = tune(), min_n = tune(), trees = 1000) %>%
  set_engine("ranger", num.threads = cores) %>%
  set_mode("classification")

rf_recipe <-
  recipe(wealth ~ ., data = pns_other)

rf_workflow <-
  workflow() %>%
  add_model(rf_mod) %>%
  add_recipe(rf_recipe)
  
rf_mod %>%
  parameters()

rf_res <-
  rf_workflow %>%
  tune_grid(val_set,
            grid = 25,
            control = control_grid(save_pred = TRUE),
            metrics = metric_set(roc_auc))

rf_res %>%
  show_best(metric = "roc_auc")

rf_best <-
  rf_res %>%
  select_best(metric = "roc_auc")

rf_res %>%
  collect_predictions()

rf_auc <-
  rf_res %>%
  collect_predictions(parameters = rf_best) %>%
  roc_curve(wealth, .pred_1:.pred_2:.pred_3:.pred_4:.pred_5) %>%
  mutate(model = "Random Forest")

bind_rows(rf_auc, lr_auc) %>%
  ggplot(aes(x = 1 - specificity, y = sensitivity, col = model)) +
  geom_path(lwd = 1.5, alpha = 0.8) +
  geom_abline(lty = 3) +
  coord_equal() +
  scale_color_viridis_d(option = "plasma", end = .6)


# second random forest

last_rf_mod <-
  rand_forest(mtry = 4, min_n = 7, trees = 1000) %>%
  set_engine("ranger", num.threads = cores, importance = "impurity") %>%
  set_mode("classification")

last_rf_workflow <-
  rf_workflow %>%
  update_model(last_rf_mod)

last_rf_fit <-
  last_rf_workflow %>%
  last_fit(splits)

last_rf_fit %>%
  collect_metrics()

last_rf_fit %>%
  pluck(".workflow", 1) %>%
  pull_workflow_fit() %>%
  vip(num_features = 20)

last_rf_fit %>%
  collect_predictions() %>%
  roc_curve(wealth, .pred_1:.pred_2:.pred_3:.pred_4:.pred_5) %>%
  
```

The following plot showed the comparison between the random forrest model and the logistic regression model

![](./Project2/rf_lr_auc.png)

From the plot, we could observe that the random forrest model had a slightly better performance than the logistic regression model.

The following showed two different random forest models. The first had higher accuracy in predicting class 1 and 4.

   **First**                                                             **Second**
![](./Project2/rf_auc.png)                                    ![](./Project2/last_rf_fit_auc.png)  



