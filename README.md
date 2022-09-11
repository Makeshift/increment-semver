Github action for getting the next semver.

Example:
========
    on: [push]
    
    jobs:
      my-increment-semver-job:
        runs-on: ubuntu-latest
        name: A job to test incrementing the version of a repository
        steps:
    
          - name: Checkout
            uses: actions/checkout@v1

          - name: Set Commit Description
            id: commit_desc
            run: |
              echo "::set-output name=value::$(git log -1 --pretty=%B)"

          - name: Regex match on commit
            uses: sunil-samtani/regex-action@v2
            id: bump
            with:
              search_string: ${{ steps.commit_desc.outputs.value }}
              regex_pattern: "major|minor|patch"

          - name: Increment Step Based on Commit Message
            id: increment-semver-patch
            uses: Makeshift/increment-semver@master
            with:
              version-level: ${{ steps.bump.outputs.first_match }}

    #     Examples
          - name: Increment Step Patch
            id: increment-semver-patch
            uses: Makeshift/increment-semver@master
            with:
              version-level: patch
    
          - name: Increment Step Minor
            id: increment-semver-minor
            uses: Makeshift/increment-semver@master
            with:
              version-level: minor
    
          - name: Increment Step Major
            id: increment-semver-major
            uses: Makeshift/increment-semver@master
            with:
              version-level: major
    
          # Use the output from the `Increment Step X` step
          - name: Get the output version
            run: |
              echo "The new patch version was ${{ steps.increment-semver-patch.outputs.version }}"
              echo "The new minor version was ${{ steps.increment-semver-minor.outputs.version }}"
              echo "The new Major version was ${{ steps.increment-semver-major.outputs.version }}"
