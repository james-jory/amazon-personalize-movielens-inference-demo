# Amazon Personalize / MovieLens - bare metal inference

## What

This project includes some basic utility scripts written in python that make it easy to subjectively evaluate the inference results of [Amazon Personalize](https://aws.amazon.com/personalize/) campaigns trained on the [MovieLens](https://grouplens.org/datasets/movielens/) "small" dataset.

## Why

The Amazon Personalize [sample notebooks](https://github.com/aws-samples/amazon-personalize-samples/tree/master/next_steps/workshops/POC_in_a_box) already do a great job of walking you through preparing and uploading the MovieLens dataset into Personalize and then training models, creating campaigns, and testing inference use-cases. The notebooks absolutely provide the best end-to-end experience. However, if you want to probe the Personalize campaigns created from the MovieLens dataset for focused and streamlined demos, these scripts may fit the bill. 

Since the Amazon Personalize console and raw API responses only include movie/item IDs, interpreting those responses against actual movie titles and user histories is painful. Hence, these bare metal scripts were developed.

## How

The scripts in this project depend on Personalize campaigns trained off the MovieLens "small" datasets already being provisioning in an AWS account that you have access to. This project does not cover the data preparation, import, and training steps. See the [notebooks](https://github.com/aws-samples/amazon-personalize-samples/tree/master/next_steps/workshops/POC_in_a_box) for these steps.

Assuming you have the 3 campaigns setup in your AWS account, you're all set to use these scripts to test them out.

### Setup

1. Go through the [notebooks](https://github.com/aws-samples/amazon-personalize-samples/tree/master/next_steps/workshops/POC_in_a_box) to train models and create campaigns in Personalize using the MovieLens "small" dataset.
1. Download the `interactions.csv` and `movies.csv` files from the SageMaker notebook instance used in the notebooks to the root of this project.
1. Pick one of the scripts and evaluate the results.

## Inference scripts

### User Personalization

The [movielens-user-personalization-demo](./movielens-user-personalization-demo.py) script will test the [User-Personalization](https://docs.aws.amazon.com/personalize/latest/dg/native-recipe-new-item-USER_PERSONALIZATION.html) backed campain.

- Pass a `user-id` on the command line or let the script randomly select a user to use as the target of personalized recommendations.
- Displays recent interactions for the user to give you a sense of the user's interests.
- Displays a summary of the most popular genres for the user based on their interaction history.
- Displays recommended movie titles and scores from the user-personalization campaign.
- Displays side-by-side comparison of the user's top genres from their interaction history with the genres from the recommended movies.

### Similar Items

The [movielens-similar-items-demo](./movielens-similar-items-demo.py) script will test the [SIMS](https://docs.aws.amazon.com/personalize/latest/dg/native-recipe-sims.html) backed campain.

- Pass an `item-id` on the command line or let the script randomly select a movie to use as the target for similar item recommendations.
- Displays the movie title and genre(s) for the selected movie.
- Displays the similar item recommendations results so you can evaluate them against the target movie.

### Personalized Ranking

The [movielens-ranking-demo](./movielens-ranking-demo.py) script will test the [Personalized-Ranking](https://docs.aws.amazon.com/personalize/latest/dg/native-recipe-search.html) backed campain.

- Pass a `user-id` on the command line or let the script randomly select a user to use as the target of personalized ranking recommendations.
- Pass `item-ids` on the command line or let the script randomly select 20 movies to rerank.
- Displays recent interactions for the user to give you a sense of the user's interests.
- Displays a summary of the most popular genres for the user based on their interaction history.
- Displays the movies reranked by Personalize for the user.
- Displays a side-by-side comparison of the unranked and reranked movies.

### Batch Recommendations

The [movielens-create-batch-input-demo](./movielens-create-batch-input-demo.py) script will generate an input file for an Amazon Personalize [batch recommendations](https://docs.aws.amazon.com/personalize/latest/dg/recommendations-batch.html) inference job and upload it to an S3 bucket.

- Specify the job type you want to create an input file for on the command line (`user-personalization`, `similar-items`, or `personalized-ranking`).
- Based on the job type, the appropriately formatted input file will be built based on a randomly selected user, item, or items (reranking).
- The generated input file will be uploaded to the S3 bucket that you specify.

You can then setup your batch inference job to run from the AWS console or via the [CreateBatchInferenceJob](https://docs.aws.amazon.com/personalize/latest/dg/API_CreateBatchInferenceJob.html) API.

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.
