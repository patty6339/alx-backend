# alx-backend

## Notes for Backend

Here’s a quick breakdown on these pagination methods:

### 1. Paginate with `page` and `page_size` Parameters
This is the most straightforward pagination method. It uses two parameters:
- **page**: Specifies which page of data to retrieve.
- **page_size**: Specifies the number of items to include per page.

**Example**: To fetch page 2 with 10 items per page:
   - `GET /items?page=2&page_size=10`
   
   This approach is simple but can lead to issues if the dataset changes frequently, as the page content might vary when items are added or removed.

### 2. Paginate with Hypermedia Metadata
This approach includes additional metadata in the response, usually following the HATEOAS (Hypermedia as the Engine of Application State) principle. The response typically includes:
- **Links to other pages**: e.g., `next`, `previous`, `first`, `last`.
- **Additional metadata**: e.g., `total_count`, `page_size`, `current_page`.

**Example**:
   ```json
   {
     "items": [...],
     "page_size": 10,
     "total_count": 100,
     "links": {
       "self": "/items?page=2",
       "next": "/items?page=3",
       "previous": "/items?page=1"
     }
   }
   ```

   This approach provides a more navigable and flexible pagination structure.

### 3. Deletion-Resilient Pagination
To handle deletion-resilient pagination, instead of relying on fixed page numbers, it’s better to use a cursor-based approach. Each item has a unique, sequential `cursor` or `ID`, and pagination is based on these identifiers rather than `page` numbers.

**Example**:
   - `GET /items?cursor=123&limit=10`
   
   In this approach:
   - The `cursor` indicates the last retrieved item.
   - `limit` specifies how many items to fetch.
   This method is stable even if items are added or removed, as it’s based on unique identifiers rather than positional indexes.
   
