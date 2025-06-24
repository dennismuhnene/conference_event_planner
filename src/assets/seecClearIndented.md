import React, { useState } from "react";
import "./ConferenceEvent.css";
import TotalCost from "./TotalCost";
import { useSelector, useDispatch } from "react-redux";
import { incrementQuantity, decrementQuantity } from "./venueSlice";

const ConferenceEvent = () => {
  const [showItems, setShowItems] = useState(false);
  const [numberOfPeople, setNumberOfPeople] = useState(1);

  const venueItems = useSelector((state) => state.venue);
  const dispatch = useDispatch();

  const remainingAuditoriumQuantity =
    3 - venueItems.find((item) => item.name === "Auditorium Hall (Capacity:200)").quantity;

  const handleToggleItems = () => {
    console.log("handleToggleItems called");
    setShowItems(!showItems);
  };

  const handleAddToCart = (index) => {
    if (
      venueItems[index].name === "Auditorium Hall (Capacity:200)" &&
      venueItems[index].quantity >= 3
    ) {
      return; // Prevent adding more than 3 Auditorium Halls
    }
    dispatch(incrementQuantity(index));
  };

  const handleRemoveFromCart = (index) => {
    if (venueItems[index].quantity > 0) {
      dispatch(decrementQuantity(index));
    }
  };

  const calculateTotalCost = (section) => {
    let totalCost = 0;
    if (section === "venue") {
      venueItems.forEach((item) => {
        totalCost += item.cost * item.quantity;
      });
    }
    return totalCost;
  };

  const venueTotalCost = calculateTotalCost("venue");

  return (
    <div className="main_container">
      {!showItems ? (
        <div className="items-information">
          <div id="venue" className="venue_container container_main">
            <div className="text">
              <h1>Venue Room Selection</h1>
            </div>
            <div className="venue_selection">
              {venueItems.map((item, index) => (
                <div className="venue_main" key={index}>
                  <div className="img">
                    <img src={item.img} alt={item.name} />
                  </div>
                  <div className="text">{item.name}</div>
                  <div>${item.cost}</div>
                  <div className="button_container">
                    {item.name === "Auditorium Hall (Capacity:200)" ? (
                      <>
                        <button
                          className={
                            item.quantity === 0 ? "btn-warning btn-disabled" : "btn-warning"
                          }
                          onClick={() => handleRemoveFromCart(index)}
                          disabled={item.quantity === 0}
                        >
                          -
                        </button>
                        <span>{item.quantity}</span>
                        <button
                          className={
                            item.quantity >= 3 ? "btn-warning btn-disabled" : "btn-warning"
                          }
                          onClick={() => handleAddToCart(index)}
                          disabled={item.quantity >= 3}
                        >
                          +
                        </button>
                      </>
                    ) : (
                      <>
                        <button
                          className={
                            item.quantity === 0 ? "btn-warning btn-disabled" : "btn-warning"
                          }
                          onClick={() => handleRemoveFromCart(index)}
                          disabled={item.quantity === 0}
                        >
                          -
                        </button>
                        <span>{item.quantity}</span>
                        <button
                          className="btn-warning"
                          onClick={() => handleAddToCart(index)}
                        >
                          +
                        </button>
                      </>
                    )}
                  </div>
                </div>
              ))}
            </div>
            <div className="total_cost">Total Cost: ${venueTotalCost}</div>
          </div>
        </div>
      ) : (
        // You can add UI here for when showItems is true, if needed
        <div>
          {/* Other UI content when showItems is true */}
        </div>
      )}
    </div>
  );
};

export default ConferenceEvent;