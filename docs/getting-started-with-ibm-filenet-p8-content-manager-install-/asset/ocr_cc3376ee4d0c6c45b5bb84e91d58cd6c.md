Each division has a variety of projects of varying sensitivity. Here are the business rules we’ll try to accommodate with our access control mechanisms. To simplify the description, we’ll consider only read access to documents; other access rights, and other object types can be accommodated in straightforward extrapolations.

+   - An author of a document has access to his or her own documents unless another item in this list applies. For example, an author has access as long as the author stays in the same division, but it is not a requirement for the author to have continued access if the author changes divisions or moves to HQ. For convenience, we refer to this as *author access*.

+   - Typical access for run of the mill documents is that divisional personnel have access to documents within the same division. Personnel from other divisions do not have access to those documents. We refer to this requirement as *divisional restriction*.

+   - There are documents that deal with sensitive areas. Examples include human resources, finance, and medical information (about HQ or divisional personnel). Only specialists in those areas have access to those documents. We refer to this requirement as *functional restriction*.

+   - Each division has some especially sensitive projects. There is no overlap in special projects between divisions. Only personnel specifically authorized for each sensitive project can get access to related documents. Those personnel might be in the division or in HQ. We refer to this restriction as *special handling*.

+   - All HQ personnel generally have access to all divisional documents except functionally restricted and special handling documents.

+   - Functional area personnel, in HQ or in the same division, generally have access to functionally restricted documents of compatible types. For example, finance personnel from HQ have access to all finance documents. Finance personnel in the Unicorn division do not have access to HQ, Ninja division, or Pirate division finance documents.

+   - Likewise, there are HQ personnel who may have access to a divisional special handling project. The list of HQ personnel varies by project, and in some cases there are none at all.

Your first inclination after reading the above requirements might be to run screaming from the room or maybe just flip pages to the next chapter of this book. Stick with us, and we’ll work through it.