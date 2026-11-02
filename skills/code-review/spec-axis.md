# Spec axis

Does the code do what the originating issue or spec asked for?

Review the change against the issue or spec it implements: the one the caller names, or the
issue a pull request closes. Failing both, use the pull request description when it defines the
expected behavior. If nothing provides a useful specification, skip this axis and say so.

Look for requirements that are missing or only partially implemented, behavior that was not
requested (scope creep), and requirements that appear implemented but are implemented incorrectly.
Cite the requirement behind each finding. For scope creep, point to the code nobody asked for.
