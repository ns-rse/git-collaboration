---
name: Schedule emails for next session
about: Instructions for scheduling emails ready for next session. To be completed
  by lead instructor
title: Schedule emails for session on [DATE-OR-SESSION-DESCRIPTION] for [INSTRUCTOR-NAME]
labels: session-setup
assignees: ''

---

Emails need to be scheduled ahead of each session.

## Emails

The following table details the emails that need scheduling. There are parameterised `.Rmd` templates in the `.emails/`
directory which should be used to generate the contents of these emails in HTML which can then be copy and pasted.

**NB** directory name is prefixed with `.` to prevent it being built as part of the website.

| Schedule        | File                        | Description                                                                                                    |
|:----------------|:----------------------------|:---------------------------------------------------------------------------------------------------------------|
| 2 Week Reminder | `setup_email.Rmd`           | Setup instructions for registered attendees as well as information about the session (venue, dates and times). |
| 4 Day Reminder  | `setup_email.Rmd`           | A reminder of the setup instructions as well as information about the session (venue, dates and times).        |
| On the Morning  | `joining_email.Rmd`         | A reminder of the course starting that day, to be scheduled one hour before the course starts.                 |
| Day After       | `feedback_survey_email.Rmd` | A request to complete the feedback survey.                                                                     |


## Templates

The templates, written in [RMarkdown][RMarkdown] are [parameterized][RMarkdown-parametrized]. This is achieved by adding
a `params:` field to the YAML header of the file with values which will then be inserted into the document. Not all
fields need parameterising across all files.

- `lead_instructor` - the lead instructors name.
- `event_day1` - date of the first session.
- `event_day1_start` - start time of first session.
- `event_day1_finish` - finish time of second session.
- `event_day2` - date of the first session.
- `event_day2_start` - start time of first session.
- `event_day2_finish` - finish time of second session.
- `survey_link` - link to [feedback survey][feedback_survey].

You should open and edit each of the email templates and update the necessary fields to reflect the details of the
session that is being run and save each file.

## Knitting

Once you have edited the templates and saved them you can render them to HTML. If you are using [RStudio][RStudio] then
you can click check the _Knit on Save_ option.

### Manually Knitting

In an R session you can from the top-level of the directory knit the file directly using...

``` r
rmarkdown::render("emails/<email-to-be-knitted>.Rmd")
```

You can also run the same command from the command line (at the top level of the repository).

``` bash
R -e "rmarkdown::render('emails/<email-to-be-knitted>.Rmd')"
```

The HTML output sits adjacent to the input files and can be opened in your web-browser and the text copied and pasted
into a draft email.

## Scheduling Emails

It is recommended to create and schedule all emails at once and use [Gmail Scheduling feature][gmail-schedule] to
schedule the emails to be sent on the appropriate days/times.

## Checklist

Complete the following checklist on scheduling each email.

- [ ] 2 Week Reminder
- [ ] 4 Day Reminder
- [ ] Morning Reminder
- [ ] Feedback Form

Once all tasks are completed please close this issue.

[feedback_survey]: https://forms.gle/NRz63kULPikoWrNz7
[gmail-schedule]: https://support.google.com/mail/answer/9214606
[RMarkdown]: https://bookdown.org/yihui/rmarkdown/
[RMarkdown-parametrized]: https://bookdown.org/yihui/rmarkdown/parameterized-reports.html
