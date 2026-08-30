"""
Problem: Remove Duplicates from a Sorted Array

Given a sorted array, remove duplicate values using
the Two Pointers technique.

Pattern:
- Two Pointers
- In-place array modification

Approach:
1. left points to the last unique element.
2. right scans through the array.
3. If nums[right] is different from nums[left],
   we found a new unique value.
4. Move left forward and store the new value there.

Time Complexity: O(n)
Extra Space Complexity: O(1)

Important:
The input array must be sorted.
"""


def remove_duplicates(nums):

    if not nums:
        return []

    left = 0

    for right in range(1, len(nums)):

        if nums[left] != nums[right]:

            left += 1

            nums[left] = nums[right]

    return nums[:left + 1]


nums = [1, 1, 2, 2, 3, 3, 4]

result = remove_duplicates(nums)

print(result)
