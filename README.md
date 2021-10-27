# assessmentLizard
// There was a problem with api/data.json that's why I had to use jsonplaceholder Json Server
import ReactPaginate from "react-paginate";
import { useEffect, useState } from "react";
import '../styles/index.css'
function App() {


const [items, setItems] = useState([]);

const [pageCount, setpageCount] = useState(0);


// Make the limit to 10 for pagination
let limit = 10;


// Gettign the data from jason file to the limit
useEffect(() => {
    const getPosts = async () => {
      const res = await fetch(
        // `http://localhost:3000/api/posts?_page=1&_limit=${limit}`
        `https://jsonplaceholder.typicode.com/photos?_page=1&_limit=${limit}`
        // `https://jsonplaceholder.typicode.com/comments?_page=1&_limit=${limit}`
      );
      const data = await res.json();
      const total = res.headers.get("x-total-count");
      setpageCount(Math.ceil(total / limit));
      setItems(data);
    };

    getPosts();
  }, [limit]);



//   Here is for next page to show more data from json
  const fetchPosts = async (currentPage) => {
    const res = await fetch(
    //   `http://localhost:3000/api/posts?_page=${currentPage}&_limit=${limit}`
      `https://jsonplaceholder.typicode.com/photos?_page=${currentPage}&_limit=${limit}`
    //   `https://jsonplaceholder.typicode.com/comments?_page=${currentPage}&_limit=${limit}`
    );
    const data = await res.json();
    return data;
  };



//   Page click handling
  const handlePageClick = async (data) => {
    console.log(data.selected);

    let currentPage = data.selected + 1;

    const commentsFormServer = await fetchPosts(currentPage);

    setItems(commentsFormServer);
    
  };


  return (
    //   Boostrap card to show the items
    <div className="container">
      <div className="row m-2">
        {items.map((item) => {
          return (
            <div key={item.albumId} className="col-sm-6 col-md-4 v my-2">
              <div className="card shadow-sm w-100" style={{ minHeight: 225 }}>
                <img src={item.thumbnailUrl} className="card-img-top" />
                <div className="card-body">
                  <h5 className="card-title text-center h2">Id :{item.albumId} </h5>
                  <h6 className="card-subtitle mb-2 text-muted text-center">
                    {item.title}
                  </h6>
                  <p className="card-text text-center">{item.email}</p>
                </div>
              </div>
            </div>
          );
        })}
      </div>


{/* Paginator number */}
      <ReactPaginate
        previousLabel={"previous"}
        nextLabel={"next"}
        breakLabel={"..."}
        pageCount={pageCount}
        marginPagesDisplayed={2}
        pageRangeDisplayed={3}
        onPageChange={handlePageClick}
        containerClassName={"pagination justify-content-center"}
        pageClassName={"page-item"}
        pageLinkClassName={"page-link"}
        previousClassName={"page-item"}
        previousLinkClassName={"page-link"}
        nextClassName={"page-item"}
        nextLinkClassName={"page-link"}
        breakClassName={"page-item"}
        breakLinkClassName={"page-link"}
        activeClassName={"active"}
      />
    </div>
  );
 


}

export default App;
