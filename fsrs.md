Binary FSRS Approach

Input for each review:

* card ID
* review timestamp
* result: true or false

Rating mapping:

* false → Again
* true → Good

Processing:

1. Load the card’s current FSRS state:

   * difficulty
   * stability
   * last-review timestamp

2. Calculate the number of days since the previous review.

3. Convert the binary result into an FSRS rating:

   * false becomes Again
   * true becomes Good

4. Pass the card state, elapsed time, and rating to FSRS.

5. FSRS updates:

   * difficulty
   * stability
   * retrievability

6. FSRS calculates the next interval using the configured desired retention.

7. Save the updated card state and next-review date.

8. Immediately display the next question.

Pseudocode:

function processAnswer(card, result, now):
elapsedDays = daysBetween(card.lastReview, now)

```
if result == true:
    rating = GOOD
else:
    rating = AGAIN

updatedCard = fsrs.review(
    cardState = card.fsrsState,
    rating = rating,
    elapsedDays = elapsedDays,
    desiredRetention = 0.90
)

save(
    difficulty = updatedCard.difficulty,
    stability = updatedCard.stability,
    nextReview = updatedCard.nextReview,
    lastReview = now
)

showNextQuestion()
```

Important behavior:

This uses the standard binary FSRS mapping. An incorrect answer is a failed recall, while a correct answer is a successful recall.

Consequences:

* false → Again records a lapse and reduces stability;
* true → Good records a successful recall and can increase stability;
* Hard and Easy are not used because a binary result does not contain enough information to distinguish those response qualities.
